Emacsw32 usage memo
===================

I have tried Emacs on windows for awhile. I search around the Internet
and put everything I found here, to make this task easier.
I've tried my best to record every source of data; however it is
always something missed and  please pointed out my errors.

The distribution used is the Emacsw32.

For copyright see COPYING(WTFPL) in the source repository or just do anything you want to do.

[Emacsw32](http://ourcomments.org/Emacs/EmacsW32.html) is a great package for those who want to test Emacs on windows platform.
The package itself comes with some command line utilities missed on windows platforms.

The DOTemacs file
----

According the [official FAQ](http://www.gnu.org/software/emacs/windows/Installing-Emacs.html), there are five possible locations 
to place the .emacs file.

>1.    If the environment variable HOME is set, use the directory it indicates.
>2.    If the registry entry `HKCU\SOFTWARE\GNU\Emacs\HOME` is set, use the directory it indicates.
>3.    If the registry entry `HKLM\SOFTWARE\GNU\Emacs\HOME` is set, use the directory it indicates. Not recommended, as it results in users sharing the same HOME directory.
>4.    If `C:\.emacs` exists, then use `C:/`. This is for backward compatibility, as previous versions defaulted to C:/ if HOME was not set.
>5.    Use the user's AppData directory, usually a directory called Application Data under the user's profile directory, the location of which varies according to Windows version and whether the computer is part of a domain. 

I choose to use AppData folder because I'm using a sharing-computer. I think HOME variables is a reasonable choice for those who are also using MinGW.

The .emacs file is the configuration file for Emacs. The prefix of ``.`` demonstrate the UNIX-style naming  convention. However, it is not possible to create a file name starting by a dot by Windows File Explorer. This seems a FEATURE of it. I suggest create it by command line. For example, to create a DOTEmacs file under `C:\`, press the *Windows Start* button and do 

```
Run ... > cmd.exe
```

Then type into the command prompt:

```
c:\> echo ; nothing > c:\.emacs
```

You may check the file content by windows command `type`:

```
c:\> type c:\.emacs
```

The DOTemacs.d directory
----

It is suggested to maintain oneself emacs plug-in under the directory `emacs.d/`, usually located under the HOME directory.
By doing so it is easy to keep the plug-in up-to-date or hold on particular version. 
However, under windows platform one can only arrange elisp files under `emacs.d/`. 

I always put my own lisp directory under `.emacs.d/` (but may not be the most correct one).

    C:\...\Application Data\.emacs.d\> mkdir site-lisp

And put any elisp I like under the `.emacs.d\site-lisp` directory.

To let emacs load your plug-ins, adding following lines in your .emacs files:

    (let ((default-directory "~/.emacs.d/site-lisp"))
      (normal-top-level-add-to-load-path '("."))
      (normal-top-level-add-subdirs-to-load-path))

So this is going to tell emacs loading every `*.el` and `*.elc` under the directory `~/emacs.d/site-lisp/` and its sub directory.

Spelling Checking
----

I can't figure out why can't I set up __ispell__. So I turn to __aspell__.

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

You may download the archive of color-theme at http://www.nongnu.org/color-theme/.
Extract every files in the archive to your local elisp directory and put the following lines into your .emacs file:

    (require 'color-theme)
    (color-theme-initialize)
    (setq color-theme-is-global t)
    (color-theme-blue-mood)         ;; Replace this line as the start function of your favorite theme.

* Press <b>M-x</b> `color-theme-select` and it will carry out a new window will a listing of all available color themes.
* Press <b>RET</b> on the name of themes then the selected theme is * applied  immediately.
* Press <b>d</b> on the name of theme will give some useful description. It also list the start function to bring out the theme.

Line numbers
----

The `line-number-mode` reports the currently line-number on the status bar only. 
Who is seeking for line numbers on the side of editor area should check the `linum-mode`.

Syntax-hightlight
----

The syntax-highlight mode in Emacs has neither "syntax" nor "highlight" in its name.
It is the minor mode ``font-lock-mode`` giving syntax highlights in Emacs. 

TABs
----
I am used to expand tabs into white spaces.  M-x customize-variables <b>RET</b> indent-tabs-mode and set it to `nil`.
One may use `<b>M-x</b> untabify` to expand the tabs into white spaces.

auctex
----

__SumatraPDF__ don't lock the viewing pdf files so it is possible to compile while SumatraPDF is viewing the desired output file. 

    ;; Change to the real path of SumatraPDF in your environment.
    (setq TeX-view-program-list '(("SumatraPDF" "c:/SumatraPDF/SumatraPDF.exe %o"))) 
    (setq TeX-view-program-selection '((output-pdf "SumatraPDF")))

Adobe Reader lacks this important features thus coppes badly with aucTeX.
  
Haskell-mode
----
Just search for ``haskell-mode``
I list my settings of haskell-mode here. It mostly comes from the example setup provided in the haskell-mode wiki page.

```
(load "~/.emacs.d/site-lisp/haskell-mode/haskell-site-file")
(add-hook 'haskell-mode-hook 'turn-on-haskell-doc-mode)

;;(add-hook 'haskell-mode-hook 'turn-on-haskell-indentation)
(add-hook 'haskell-mode-hook 'turn-on-haskell-indent)
;;(add-hook 'haskell-mode-hook 'turn-on-haskell-simple-indent)

;; If ghci is not under usual position.
;;(setq haskell-program-name "/some/where/ghci.exe")
;;  (setq haskell-program-name
;;        (if (eq system-type 'cygwin)
;;            "/cygdrive/c/ghc/ghc-6.8.1/bin/ghcii.sh"
;;          "c:/ghc/ghc-6.8.1/bin/ghci.exe"))

;; Hook-adding pattern.
;; Starting font-lock-mode and linum-mode for haskell source files.
(add-hook 'haskell-mode-hook
          '(lambda ()
             (progn
               (font-lock-mode)
               (linum-mode))))
;; REPL of Haskell. 
(require 'inf-haskell)
```

Emacs-style key strokes in Other IDEs
----

It is possible to edit like if you are in Emacs in other IDEs. This may make programming in cumbersome IDEs more comfortable. Check the following sites.

* http://marketplace.eclipse.org/node/886
* http://blogs.msdn.com/b/visualstudio/archive/2010/09/01/emacs-emulation-extension-now-available.aspx

References
----
* http://www.emacswiki.org/
* http://www.emacswiki.org/cgi-bin/wiki/EmacsW32
