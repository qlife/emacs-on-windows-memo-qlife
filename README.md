Emacsw32 usage memo
===================

This document gives a tutorial about setting up Emacs on Windows platform.
The distribution used is the Emacsw32.

For copyright see COPYING(WTFPL) in the same repository or just do anything you want to do.

[Emacsw32](http://ourcomments.org/Emacs/EmacsW32.html) is a great package for those who want test Emacs on windows platform.
The package itself comes with some missed command line utilities, this is much convenient.

The DOTEmacs file
-----------------

According the [official FAQ](http://www.gnu.org/software/emacs/windows/Installing-Emacs.html), there are five possible locations 
to place the .emacs file.

1.    If the environment variable HOME is set, use the directory it indicates.
2.    If the registry entry `HKCU\SOFTWARE\GNU\Emacs\HOME` is set, use the directory it indicates.
3.    If the registry entry `HKLM\SOFTWARE\GNU\Emacs\HOME` is set, use the directory it indicates. Not recommended, as it results in users sharing the same HOME directory.
4.    If `C:\.emacs` exists, then use `C:/`. This is for backward compatibility, as previous versions defaulted to C:/ if HOME was not set.
5.    Use the user's AppData directory, usually a directory called Application Data under the user's profile directory, the location of which varies according to Windows version and whether the computer is part of a domain. 

I choose using AppData folder because I'm using a sharing-computer. I think HOME variables is a reasonable choice for those who are also using MinGW.

The .emacs file is the configuration file for Emacs. The name convention is UNIX style. However, it is not possible to create a file name starting by a dot by Windows File Explorer. This seems a FEATURE of it.
I suggest create it by command line. For example, to create a DOTEmacs file under `C:\`, press the *Windows Start* button and do `Run ... > cmd.exe`. Then type into the command prompt:
```
c:\> echo ; nothing > c:\.emacs
```
You may check the file content by windows command `type`:
```
c:\> type c:\.emacs
```





