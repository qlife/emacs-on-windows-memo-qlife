Emacsw32 usage memo
===================

I have taste Emacs on windows for awhile. I googled around and put everything I found here, to make this task easier.
I've tried my best to record any sources; however it is always something missed, please pointed out my errors.

The distribution used is the Emacsw32.

For copyright see COPYING(WTFPL) in the same repository or just do anything you want to do.

[Emacsw32](http://ourcomments.org/Emacs/EmacsW32.html) is a great package for those who want test Emacs on windows platform.
The package itself comes with some missed command line utilities, this is much convenient.

The DOTemacs file
----

According the [official FAQ](http://www.gnu.org/software/emacs/windows/Installing-Emacs.html), there are five possible locations 
to place the .emacs file.

>1.    If the environment variable HOME is set, use the directory it indicates.
>2.    If the registry entry `HKCU\SOFTWARE\GNU\Emacs\HOME` is set, use the directory it indicates.
>3.    If the registry entry `HKLM\SOFTWARE\GNU\Emacs\HOME` is set, use the directory it indicates. Not recommended, as it results in users sharing the same HOME directory.
>4.    If `C:\.emacs` exists, then use `C:/`. This is for backward compatibility, as previous versions defaulted to C:/ if HOME was not set.
>5.    Use the user's AppData directory, usually a directory called Application Data under the user's profile directory, the location of which varies according to Windows version and whether the computer is part of a domain. 

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

The DOTemacs.d directory
----

Suggest maintain your elisp plugins _locally_ .

Spelling Checking
----

I failed to setting up with _ispell_.

However, on the windows platform _aspell_ is very easy to install. You may find windows port of aspell and dictionaries by http://aspell.net/.
Add the location of _aspell.exe_ into your PATH environment variables, and put the following lines into your .emacs file.

    ;; ispell comes before auctex always

    (setq ispell-program-name "aspell")
    (setq ispell-list-command "list")
    (setq ispell-extra-args '("--sug-mode=ultra"))


You may use _flyspell-mode_ to interactive to spell checkers. 
When flyspell-mode is enable, misspelling words are marked as red and underline. 
Use the keystrokes <b>M-$</b> to obtain suggested correction.  

color-theme
----

auctex
----

slime
----

C/C++ programming with MinGW
----

Backup file
----

References
----
* http://www.emacswiki.org/
* http://www.emacswiki.org/cgi-bin/wiki/EmacsW32