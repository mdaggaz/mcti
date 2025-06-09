# Dynamische Analyse mit x32dbg

## 1. Vorbereitung der Ausführung – EIP setzen

Nach dem Laden der Datei `full_sc.exe` im Debugger landet der Initialwert des EIP-Registers auf folgender Adresse:

```
EIP = 7C91120F
```

Diese Adresse befindet sich im Bereich der `ntdll.dll` und nicht im tatsächlichen Codebereich des Programms. Um die tatsächliche Ausführung des Malware-Codes zu analysieren, müssen wir den EIP manuell auf den Original-Entry-Point des Programms setzen:

```
EIP → 00401000
```

### 1.1 Speicherrechte setzen

Bevor wir den Code unter `00401000` ausführen, müssen wir sicherstellen, dass dieser Speicherbereich die richtigen Zugriffsrechte hat. Dafür setzen wir die Speicherrechte wie folgt:

```
→ Rechtsklick im Memory Map (00401000) → Set Page Memory Rights → Full Access (R/W/X)
```

Dadurch erlauben wir das Lesen, Schreiben und Ausführen des Codes im angegebenen Speicherbereich.

## 2. Parameter vorbereiten

Nachdem wir den EIP auf `00401000` gesetzt haben, führen wir den Code schrittweise mit **Step Over (F8)** aus, bis wir folgende Anweisungen erreichen:

```
00401020 | 6A 00              | push 0
00401022 | 68 80000000        | push 80
00401027 | 6A 03              | push 3
00401029 | 6A 00              | push 0
0040102B | 6A 01              | push 1
0040102D | 68 00000080        | push 80000000
00401032 | 68 02104000        | push full_sc.401002 → "C:\sc.pdf"
00401037 | E8 D5DA7476        | call 76B4EB11
```

### 2.1 Analyse des Aufrufs bei 00401037

Man beobachtet, dass bei Adresse `00401037` ein Aufruf an `76B4EB11` erfolgt. Dieser ist falsch und muss durch einen direkten Aufruf von `CreateFileA` ersetzt werden.

```c
HANDLE CreateFileA(
  [in]           LPCSTR                lpFileName,
  [in]           DWORD                 dwDesiredAccess,
  [in]           DWORD                 dwShareMode,
  [in, optional] LPSECURITY_ATTRIBUTES lpSecurityAttributes,
  [in]           DWORD                 dwCreationDisposition,
  [in]           DWORD                 dwFlagsAndAttributes,
  [in, optional] HANDLE                hTemplateFile
);
```

## 3. Payload entschlüsseln

Nach dem Aufruf kehrt die Ausführung zum folgenden Code zurück, wo eine Dekodierschleife beginnt:

```
00401048 | 5E              | pop esi
00401049 | 56              | push esi
0040104A | 8BFE            | mov edi, esi
0040104C | AC              | lodsb
0040104D | 66:2D 6100      | sub ax, 0061h
00401051 | 66:C1E0 04      | shl ax, 4
00401055 | 66:8BD0         | mov dx, ax
00401058 | AC              | lodsb
00401059 | 2C 61           | sub al, 61h
0040105B | 66:03C2         | add ax, dx
0040105E | AA              | stosb
0040105F | 49              | dec ecx
00401060 | 75 EA           | jne 0040104C
00401062 | C3              | ret
00401063 | E8 E0FFFFFF     | call 00401048
```

### 3.1 Wichtiger Hinweis zur Schleife

Die Schleife zwischen `0040104C` und `00401060` dekodiert den Payload-Teil des Malware-Samples. Um eine manuelle Ausführung der langen Schleife zu vermeiden, setzen wir **einen Breakpoint auf Adresse `00401068`**, direkt nach dem `call 00401048`:

```
00401068 | 55              | push ebp
```

Danach wird die Ausführung mit **F9 (Run)** fortgesetzt. Dies ermöglicht es, die vollständige Entschlüsselung des Payloads automatisch durchzuführen und danach mit der Analyse des entschlüsselten Codes fortzufahren.

## 4. API-Adressen finden

Nach erfolgreicher Entschlüsselung folgt ein Abschnitt, der die Adressen von wichtigen Windows-APIs dynamisch auflöst. Der komplette Codeabschnitt sieht folgendermaßen aus:

```
00401068 | 55                       | push ebp
00401069 | 8BEC                     | mov ebp,esp
0040106B | 83EC 5C                  | sub esp,5C
0040106E | 53                       | push ebx
0040106F | 56                       | push esi
00401070 | 57                       | push edi
00401071 | C745 F0 11000000         | mov dword ptr ss:[ebp-10],11
00401078 | E9 1E010000              | jmp full_sc.40119B
0040107D | 8F45 FC                  | pop dword ptr ss:[ebp-4]
00401080 | 64:A1 30000000           | mov eax,dword ptr fs:[30]
00401086 | 8B40 0C                  | mov eax,dword ptr ds:[eax+C]
00401089 | 8B70 1C                  | mov esi,dword ptr ds:[eax+1C]
0040108C | AD                       | lodsd
0040108D | 8B58 08                  | mov ebx,dword ptr ds:[eax+8]
00401090 | 895D EC                  | mov dword ptr ss:[ebp-14],ebx
00401093 | 8B4D F0                  | mov ecx,dword ptr ss:[ebp-10]
00401096 | 8B7D FC                  | mov edi,dword ptr ss:[ebp-4]
00401099 | 51                       | push ecx
0040109A | 8B45 EC                  | mov eax,dword ptr ss:[ebp-14]
0040109D | 8B70 3C                  | mov esi,dword ptr ds:[eax+3C]
004010A0 | 8B7406 78                | mov esi,dword ptr ds:[esi+eax+78]
004010A4 | 03F0                     | add esi,eax
004010A6 | 56                       | push esi
004010A7 | 8B76 20                  | mov esi,dword ptr ds:[esi+20]
004010AA | 03F0                     | add esi,eax
004010AC | 33C9                     | xor ecx,ecx
004010AE | 49                       | dec ecx
004010AF | 41                       | inc ecx
004010B0 | AD                       | lodsd
004010B1 | 0345 EC                  | add eax,dword ptr ss:[ebp-14]
004010B4 | 33DB                     | xor ebx,ebx
004010B6 | 0FBE10                   | movsx edx,byte ptr ds:[eax]
004010B9 | 3AD6                     | cmp dl,dh
004010BB | 74 08                    | je full_sc.4010C5
004010BD | C1CB 0D                  | ror ebx,0D
004010C0 | 03DA                     | add ebx,edx
004010C2 | 40                       | inc eax
004010C3 | EB F1                    | jmp full_sc.4010B6
004010C5 | 3B1F                     | cmp ebx,dword ptr ds:[edi]
004010C7 | 75 E6                    | jne full_sc.4010AF
004010C9 | 5E                       | pop esi
004010CA | 8B5E 24                  | mov ebx,dword ptr ds:[esi+24]
004010CD | 035D EC                  | add ebx,dword ptr ss:[ebp-14]
004010D0 | 66:8B0C4B                | mov cx,word ptr ds:[ebx+ecx*2]
004010D4 | 8B5E 1C                  | mov ebx,dword ptr ds:[esi+1C]
004010D7 | 035D EC                  | add ebx,dword ptr ss:[ebp-14]
004010DA | 8B048B                   | mov eax,dword ptr ds:[ebx+ecx*4]
004010DD | 0345 EC                  | add eax,dword ptr ss:[ebp-14]
004010E0 | AB                       | stosd
004010E1 | 59                       | pop ecx
004010E2 | E2 B5                    | loop full_sc.401099
```
### Erklärung 
Dieser Code implementiert eine Schleife, die:
  - über einen Speicherbereich iteriert, der Hashes von Ziel-API-Namen enthält,
  - die exportierten Funktionen einer geladenen DLL durchläuft,
  - von jedem Funktionsnamen den Hash berechnet,
  - diesen mit dem Ziel-Hash vergleicht,
  - bei Übereinstimmung die Adresse der passenden API speichert.
Dies ist eine gängige Technik in Malware zur Umgehung statischer Analyse und zur dynamischen Auflösung benötigter Windows-API-Funktionen.

## 5. Dateigröße ermitteln

Nach der dynamischen Auflösung der APIs wird im nächsten Schritt die Funktion `GetFileSize` indirekt über einen Funktionszeiger aufgerufen. Die relevante Code-Sektion beginnt ab Adresse `004010E4`:

```
004010E4 | 33C0               | xor eax, eax
004010E6 | 8945 E8            | mov dword ptr ss:[ebp-18], eax
004010E9 | 8945 E4            | mov dword ptr ss:[ebp-1C], eax
004010EC | 8345 E4 04         | add dword ptr ss:[ebp-1C], 4
004010F0 | 6A 00              | push 0                       ; lpFileSizeHigh = NULL
004010F2 | 8B45 E4            | mov eax, dword ptr ss:[ebp-1C]
004010F5 | 50                 | push eax                    ; hFile
004010F6 | 8B4D FC            | mov ecx, dword ptr ss:[ebp-4]
004010F9 | FF51 10            | call dword ptr ds:[ecx+10]  ; indirekter Aufruf → GetFileSize
```

Diese Aufrufweise mit call [ecx+10h] deutet stark darauf hin, dass es sich um einen Aufruf der API GetFileSize handelt. Die Parameter passen exakt zur Signatur:
``` 
DWORD GetFileSize(
  HANDLE  hFile,
  LPDWORD lpFileSizeHigh
);
```
Die Analyse per Step Over (F8) zeigt, dass der Malware-Code mehrere Versuche durchführt, um eine gültige Dateigröße zu ermitteln. Bei insgesamt 7 Ausführungen der Instruktion 004010F9, wurde der Rückgabewert EAX einmal 0x00000000, dann 0xFFFFFFFF, und schließlich ein stabiler Wert EAX = 0x000120A9 festgestellt, was einer Dateigröße von 73897 Bytes entspricht.
Diese Datei ist vermutlich der verschlüsselte Payload (sc.pdf) und dient als Grundlage für die nächsten Verarbeitungsschritte.
