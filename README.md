# 1 PyQT5 Programm in exe umwandeln mit Anaconda

1.	Code vorbereiten
a.	Code aufräumen: Nicht benötigte Dateien löschen, nicht benötigte imports rausnehmen, nur Sachen importieren, die verwendet werden
b.	Alle Dateien und Unterordner, die für das Programm gebraucht werden direkt in einem Ordner zusammenfassen („Programmordner“)
2.	PyInstaller installieren
a.	Anaconda starten
b.	Commanda Konsole auswählen
c.	In Ordner „C.\ProgramData\Anaconda3\Scripts“ wechseln (cd)
d.	Pyinstaller installieren: „conda install -c conda-forge pyinstaller“
3.	Programmordner kopieren nach „C.\ProgramData\Anaconda3\Scripts“
4.	Falls noch keine .spec-Datei vorhanden: .spec-Datei erzeugen lassen
a.	Pyinstaller ausführen: „pyinstaller ProgrammOrdner//MeinProgramm.py“
i.	Falls Fehlermeldung mit enum24: deinstallieren: „pip uninstall -y enum34“
5.	.spec-Datei spezifizieren
a.	Verweise auf externe Dateien im Feld data hinzufügen
i.	Dateien
data = ‘Programmordner\\MeineDatei.py‘, ‘zielordner‘), (‘Programmordner\\MeineDatei.txt’, ‘zielordner’)]
Falls sich Datei auf gleicher Ebene wie .EXE sein soll, dann zielordner = ‘.’ setzen (WICHTIG: \\ statt \ verwenden)
ii.	Gesamte Ordnerinhalte
data = [(‘Programmordner\\MeinOrdner’, ‘MeinOrdner’)]

b.	Falls Fehlermeldung „ModuleNotFoundError: No module named 'pkg_resources.py2_warn'“  hidden imports anpassen
hiddenimports=['pkg_resources.py2_warn']
[https://github.com/pypa/setuptools/issues/1963]
c.	Falls Fehlermeldung “ModuleNotFoundError: sklearn”
'sklearn.neighbors.quad_tree', 'sklearn.tree._utils', 'sklearn.neighbors.typedefs', 'sklearn.utils._cython_blas'  hidden imports anpassen
hiddenimports=['pkg_resources.py2_warn', 'sklearn.neighbors.quad_tree', 'sklearn.tree._utils', 'sklearn.neighbors.typedefs', 'sklearn.utils._cython_blas']
d.	Ggf. weitere Anpassungen treffen
i.	Im Bereich EXE: „console=False“, falls keine Konsole angezeigt werden soll
e.	Falls Fehlermeldung „pyimod03_importers.py line 623 in exec_module“
i.	pyinstaller -F --paths C:\Users\lixib\Desktop\Code\venv\Lib\site-packages\scipy\extra-dll pdf_num_detect.py


### Beispiel .spec-Datei

# -*- mode: python ; coding: utf-8 -*-

block_cipher = None

```
a = Analysis(['Analysator_Recording\\Aufnahme_PROBE.py'],
             pathex=['C:\\ProgramData\\Anaconda3\\Scripts'],
             binaries=[],
             datas=[('Analysator_Recording\\MainWindow.py', '.'),
			 ('Analysator_Recording\\read_balance.py', '.'),
			 ('Analysator_Recording\\read_camera.py', '.'),
			 ('Analysator_Recording\\settings', 'settings'),
			 ('Analysator_Recording\\img', 'img')],
             hiddenimports=['pkg_resources.py2_warn'],
             hookspath=[],
             runtime_hooks=[],
             excludes=[],
             win_no_prefer_redirects=False,
             win_private_assemblies=False,
             cipher=block_cipher,
             noarchive=False)
pyz = PYZ(a.pure, a.zipped_data,
             cipher=block_cipher)
exe = EXE(pyz,
          a.scripts,
          [],
          exclude_binaries=True,
          name='Aufnahme_PROBE',
          debug=False,
          bootloader_ignore_signals=False,
          strip=False,
          upx=True,
          console=True )
coll = COLLECT(exe,
               a.binaries,
               a.zipfiles,
               a.datas,
               strip=False,
               upx=True,
               upx_exclude=[],
               name='Aufnahme_PROBE')
```
