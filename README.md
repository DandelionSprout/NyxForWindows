# Nyx for Windows

The three main files in this repo, [starter.py](https://raw.githubusercontent.com/DandelionSprout/NyxForWindows/main/starter.py), [header.py](https://raw.githubusercontent.com/DandelionSprout/NyxForWindows/main/header.py), and [tracker.py](https://raw.githubusercontent.com/DandelionSprout/NyxForWindows/main/tracker.py), can be used to patch and get the `pip` version of Nyx 2.1.0 (A monitor tool for Tor) to run natively on Windows 11, Windows 10, or Windows 8.

This is done by simply removing the scripts' dependency on the `get_uid` attribute, and replacing instances of `os.uname` with `platform.name`, in order to run to completion. Not exactly a strategy that expert coders would've used (which I'm probably not), but it works in this case.

### Prerequisites

* Python 3 downloaded from https://www.python.org/downloads/ (I've tested with Python 3.9. Using the Microsoft Store versions has not yet been tested, since folder filepaths used by Microsoft Store apps tend to change *often*.)
* An up-to-date version of `pip` or `pip3` within Python 3
* (Optional but recommended) [Voidtools Everything](https://www.voidtools.com/) to quickly find file locations

### My procedure in-so-far as I can remember it

1) In accordance with https://nyx.torproject.org/#download, open PowerShell and run `pip install nyx`
2) Once it has been installed, run `nyx`
3) If you encounter the error `ImportError: No module named '_curses'`: Install *windows-curses* with `pip install windows-curses`. (Credit: [Bruno Ranieri](https://stackoverflow.com/questions/35850362/importerror-no-module-named-curses-when-trying-to-import-blessings))
4) If you encounter an error about not being able to find `control_auth_cookie`: My understanding is that this can occur if Tor has been placed in `C:\Program Files`, since Tor on Windows lacks admin rights and will save new files to `%LOCALAPPDATA%\VirtualStore` instead. This can be fixed by pointing to the file's actual location in `torrc` with `CookieAuthFile ______________\Data\control_auth_cookie`
5) If you receive an error about `get_uid`: Right-click on [header.py](https://raw.githubusercontent.com/DandelionSprout/NyxForWindows/main/header.py), choose `Save As...`, and overwrite the default `header.py` file. In my case with Python 3.9, it was in `%LOCALAPPDATA%\Programs\Python\Python39\Lib\site-packages\nyx\panel\header.py`
6) If you receive an error about `uname`: Right-click on [starter.py](https://raw.githubusercontent.com/DandelionSprout/NyxForWindows/main/starter.py), choose `Save as...`, and overwrite the default `starter.py` file. In my case with Python 3.9, it was in `%LOCALAPPDATA%\Programs\Python\Python39\Lib\site-packages\nyx\starter.py`
7) If the "Graph / Log" page gets clogged with `[NYX_Warning] Event listener raised an uncaught exception (module 'os' has no attribute 'uname'): BW x x` every 1-2 seconds: Right-click on [tracker.py](https://raw.githubusercontent.com/DandelionSprout/NyxForWindows/main/tracker.py), choose `Save as...`, and overwrite the default `tracker.py` file. In my case with Python 3.9, it was in `%LOCALAPPDATA%\Programs\Python\Python39\Lib\site-packages\nyx\tracker.py`

## Known issues

None as of 9th of February 2022.
