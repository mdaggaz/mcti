# Malware Analyse Etappen

## 1. Einstiegspunkt setzen
Das EIP (Entry Instruction Pointer) wurde manuell auf die Adresse `0x00401000` gesetzt.

## 2. Erste Beobachtungen bei `0x00401037`
Wir haben mehrmals den Befehl `Step Over` verwendet, bis wir die Adresse `0x00401037` erreicht haben.

**➤ Was sich geändert hat:**
- Der Funktionsname wurde manuell zu `CreateFileA` umbenannt, um die Lesbarkeit und Analyse zu erleichtern.
- Der Wert von `EAX` hat sich von `0x00000000` auf `0x000000EC` geändert.

## 3. Blockierung bei `0x0040105E`
Wir haben weiterhin `Step Over` verwendet, bis wir die Adresse `0x0040105E` erreicht haben. Dort blieb die Ausführung jedoch blockiert und konnte nicht fortgesetzt werden. Erst nachdem im Speicher vollständiger Zugriff (**Full Access**) auf die Adresse `0x0040105E` gewährt wurde, konnte die Analyse fortgesetzt werden.

**➤ Was sich geändert hat:**
- `EAX = 0x00000CB5` ⇒ `AL = 0xB5` (niedrigstes Byte von `AX`)
- Wert `0xB5` wurde in den Speicher geschrieben
- `EDI = 0x00401069` (Zieladresse)
- `ESI = 0x0040106A` (Quelladresse oder nächste Instruktion)
- `ECX = 0x017C` (380 dezimal)

## 4. Analyse der Schleife mit STOSB
Wir haben weiterhin `Step Over` verwendet, bis wir erneut die Adresse `0x0040105E` erreicht haben, an der die Instruktion `STOSB` (Opcode `AA`) ausgeführt wurde.

Diese Adresse liegt inmitten einer Schleife:

```
0040104C | AC            | LODSB        ; lädt ein Byte von [ESI] in AL
0040104D | 66:2D 6100    | SUB AX, 0x61
00401051 | 66:C1E0 04    | SHL AX, 4
00401055 | 66:8BD0       | MOV DX, AX
00401058 | AC            | LODSB
00401059 | 2C 61         | SUB AL, 0x61
0040105B | 66:03C2       | ADD AX, DX
0040105E | AA            | STOSB
0040105F | 49            | DEC ECX
00401060 | 75 EA         | JNE 0040104C
```

**➤ Interpretation:**
- Die Schleife liest zwei kodierte Zeichen pro Durchlauf
- Rechnet daraus ein Byte (4 Bit + 4 Bit)
- Schreibt dieses Byte in einen Speicherpuffer (`[EDI]`)
- Wiederholt sich 380-mal (`ECX = 0x017C`) → ergibt 760 Zeichen dekodierter Payload

## 5. Entschlüsselung und Analyse der Payload bei `0x0040106A`
Wir haben weiterhin `Step Over` verwendet, bis wir die Adresse `0x0040106A` erreicht haben. Dort wurde die Instruktion `IN AL, DX` (Opcode: `EC`) ausgeführt.

**➤ Was sich geändert hat:**
- Die Instruktion `IN AL, DX` liest ein Byte von einem I/O-Port, dessen Adresse im Register `DX` gespeichert ist, und speichert es in `AL`.

**➤ Vorgehen:**
- Wir haben die verschlüsselte Zeichenkette ab `ESI = 0x0040106A` extrahiert
- Mit folgendem Python-Skript wurde sie entschlüsselt:

```python
encoded = "Yl_mYd_mfmfdfgfh]hef`abbaaaaaa_jboabaaaaYpef...YaaaaaYle"

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

- Danach wurde die Datei `decrypted_output.bin` mit `ndisasm` disassembliert.

**➤ Disassembly zeigt unter anderem:**
```asm
00000017  64A13000      mov eax, fs:[0x30] ; Zugriff auf PEB
...
```

**➤ Interpretation:**
- Zugriff auf das `Process Environment Block (PEB)` zur Ermittlung der Basisadressen geladener DLLs
- Typische Vorgehensweise für Shellcode, um `LoadLibrary` und `GetProcAddress` aufzurufen
- Dies deutet auf einen **staged Shellcode** hin, der dynamisch weitere Funktionen nachlädt und ausführt
