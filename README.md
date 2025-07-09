# Nyx for Windows

The four main files in this repo, [starter.py](https://raw.githubusercontent.com/DandelionSprout/NyxForWindows/main/starter.py), [header.py](https://raw.githubusercontent.com/DandelionSprout/NyxForWindows/main/header.py), and [tracker.py](https://raw.githubusercontent.com/DandelionSprout/NyxForWindows/main/tracker.py), can be used to patch and get the `pip` version of Nyx 2.1.0 (A monitor tool for Tor) to run natively on Windows 11, Windows 10, or Windows 8.

This is done by simply removing the scripts' dependency on the `get_uid` attribute, and replacing instances of `os.uname` with `platform.uname` (Credit: [lunaken](https://github.com/lunaken)), in order to run to completion. Not exactly a strategy that expert coders would've used (which I'm probably not), but it works in this case.

### Prerequisites

* Python 3 downloaded from https://www.python.org/downloads/ or from Microsoft Store.
* An up-to-date version of `pip` or `pip3` within Python 3.
* (Optional but recommended) [Voidtools Everything](https://www.voidtools.com/) to quickly find file locations.

### My procedure in-so-far as I can remember it

1) In accordance with https://nyx.torproject.org/#download: Open PowerShell and run `pip install nyx windows-curses` (Credit for recommending `windows-curses`: [Bruno Ranieri](https://stackoverflow.com/questions/35850362/importerror-no-module-named-curses-when-trying-to-import-blessings))
2) Once it has been installed, run `nyx`
3) **If** you encounter an error about not being able to find `control_auth_cookie`: My understanding is that this can occur if Tor has been placed in `C:\Program Files`, since Tor on Windows lacks admin rights and will save new files to `%LOCALAPPDATA%\VirtualStore` instead. This can be fixed by pointing to the file's actual location in `torrc` with `CookieAuthFile ______________\Data\control_auth_cookie`
4) **If** you receive an error about `get_uid`: Right-click on [header.py](https://raw.githubusercontent.com/DandelionSprout/NyxForWindows/main/header.py), choose `Save As...`, and overwrite the default `header.py` file. In my case with Python 3.9, it was in `%LOCALAPPDATA%\Programs\Python\Python39\Lib\site-packages\nyx\panel\header.py`
5) **If** you receive an error about `uname`: Right-click on [starter.py](https://raw.githubusercontent.com/DandelionSprout/NyxForWindows/main/starter.py), choose `Save as...`, and overwrite the default `starter.py` file. In my case with Python 3.9, it was in `%LOCALAPPDATA%\Programs\Python\Python39\Lib\site-packages\nyx\starter.py`
6) **If** the "Graph / Log" page gets clogged with `[NYX_Warning] Event listener raised an uncaught exception (module 'os' has no attribute 'uname'): BW x x` every 1-2 seconds: Right-click on [tracker.py](https://raw.githubusercontent.com/DandelionSprout/NyxForWindows/main/tracker.py), choose `Save as...`, and overwrite the default `tracker.py` file. In my case with Python 3.9, it was in `%LOCALAPPDATA%\Programs\Python\Python39\Lib\site-packages\nyx\tracker.py`
7) **If** trying to view the config list results in `AttributeError: module 'inspect' has no attribute 'getargspec'. Did you mean: 'getargs'?`: Right-click on [panel/\_\_init\_\_.py](https://raw.githubusercontent.com/DandelionSprout/NyxForWindows/main/panel/__init__.py), choose `Save as...`, and overwrite the `__init__.py` file in the **panel** subfolder. **D O N O T** replace the file in the main folder (Credits for clueing me on to the fix after I websearched it: [zou3519](https://github.com/pytorch/pytorch/issues/15344))

## Known issues

The setup was initially designed with Python 3.9 in mind. While it does work on later Python versions, some non-critical compatibility bugs are present with Python 3.13.

* 17 June 2025: When launching Nyx in PowerShell 7 in Windows Terminal (as of Terminal's v1.22.11141.0), the colors can look a bit warped. This is worked around by dragging the window in/out slightly each time after launch. The standalone PowerShell 7.5.x program is unaffected.
* 15 June 2025: When using Python 3.13, Nyx is prone to often hang during startup for a multitude of reasons, mostly the first few times Nyx is launched (or is launched after a Tor update). Using `nyx --debug [Filepath without these brackets]` is recommended when expecting such hangs.
* * Nyx can hang for more than 25 minutes while "PROTOCOLINFO 1" and later "AUTHCHALLENGE SAFECOOKIE" are processed. If only those 2 are processed, then it will however correctly start Nyx. Whether this is specific to Python 3.13 or not is unclear.
* * Nyx can softlock if after initially hung for minutes when processing "PROTOCOLINFO 1" and "AUTHCHALLENGE SAFECOOKIE", it then attempts to process "AUTHENTICATE", and does not seem to progress past there.
* * Nyx can sometimes, but far from always, throw an error soon after startup if it can't find a file called `ps`.

### Fixed

* 9 July 2025: Viewing the config file in Nyx in Python 3.13 would crash Nyx after circa 2 seconds. An additional extra file has been added to this repo for replacements of `nyx\panel\__init__.py`.
* 15 June 2025: `nyx --debug [Filepath without these brackets]` would fail in Python 3.13 due to `platform.dist` in starter.py not being supported in Python 3.13. This was changed to `platform.system` in a repo update later that day.

## Other details

Screenshot of the program in use (with censored parts marked in orange):

![Nyx for Windows in use](https://github.com/user-attachments/assets/147b0613-d543-4186-9425-c39a098b28a8)
