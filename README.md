urxvt-scripts
=============

A small collection of perl extensions for the [rxvt-unicode][urxvt] terminal
emulator.

[urxvt]: http://software.schmorp.de/pkg/rxvt-unicode.html

Installation
------------

Place the scripts you want to install in `$HOME/.urxvt/ext/` folder.

Note: If your urxvt version is older than 9.14 you will have to add the path to
the `URxvt.perl-lib` resource.

See the following sections for information on how to enable the scripts or set
script-specific options and keyboard mappings in your `~/.Xresources`.

clipboard
---------
Author:  Bert Muennich  
License: GPLv2  
URL:     http://www.github.com/muennich/urxvt-perls

**DEPRECATED**

Since version 9.20 rxvt-unicode natively supports copying to and pasting from
the CLIPBOARD buffer with the `Ctrl-Meta-c` and `Ctrl-Meta-v` key bindings. The
`clipboard.autocopy` setting is provided by the `selection_to_clipboard`
extension shipped with rxvt-unicode.

Use keyboard shortcuts to copy the selection to the clipboard and to paste the
clipboard contents (optionally escaping all special characters).

After installing, enable the extension by adding the following line in your
`~/.Xresources`:

```
URxvt.perl-ext-common: ...,clipboard
```

Then add some keybindings:

```
URxvt.keysym.M-c:   perl:clipboard:copy
URxvt.keysym.M-v:   perl:clipboard:paste
URxvt.keysym.M-C-v: perl:clipboard:paste_escaped
```

Options:

-   `URxvt.clipboard.autocopy`: if set to true, the clipboard is automatically
    updated whenever the PRIMARY selection changes

You can also overwrite the system commands to use for copying/pasting. The
default ones are:

```
URxvt.clipboard.copycmd:  xsel -ib
URxvt.clipboard.pastecmd: xsel -ob
```

If you prefer xclip, then put these lines in your `~/.Xresources`:

```
URxvt.clipboard.copycmd:  xclip -i -selection clipboard
URxvt.clipboard.pastecmd: xclip -o -selection clipboard
```

On Mac OS X, put these lines in your `~/.Xresources`:

```
URxvt.clipboard.copycmd:  pbcopy
URxvt.clipboard.pastecmd: pbpaste
```

The use of the functions should be self-explanatory!

font-size
---------
Author:  Jan Larres  
License: MIT License  
URL:     https://github.com/majutsushi/urxvt-font-size  
Based on:
-  https://github.com/dave0/urxvt-font-size (David O'Neill)
-  https://github.com/noah/urxvt-font (Noah K. Tilton)

Change the font size on the fly with keyboard shortcuts. It has the following
features:

-   Supports both xft and X11 fonts; X11 fonts work in both full form and as
    aliases.
-   Supports all four font settings: `font`, `boldFont`, `italicFont` and
    `boldItalicFont` and changes them in accordance with the base font (the
    first one from `font`).
-   Can apply the font change globally for the whole server, so that new
    terminals will inherit the same size, and even save it to `~/.Xresources` to
    be able to survive a reboot.
-   Should work even with complicated font setups like the example in the urxvt
    man-page.

After installing, enable the extension by adding the following line in your
`~/.Xresources`:

```
URxvt.perl-ext-common: ...,font-size
```

Then add some keybindings:

```
URxvt.keysym.C-Up:     font-size:increase
URxvt.keysym.C-Down:   font-size:decrease
URxvt.keysym.C-S-Up:   font-size:incglobal
URxvt.keysym.C-S-Down: font-size:decglobal
```

Note that for urxvt versions older than 9.21 the resources have to look like this:

```
URxvt.keysym.C-Up:     perl:font-size:increase
URxvt.keysym.C-Down:   perl:font-size:decrease
URxvt.keysym.C-S-Up:   perl:font-size:incglobal
URxvt.keysym.C-S-Down: perl:font-size:decglobal
```

The following functions are supported:

-   `increase`/`decrease`: Increase or decrease the font size of the current
    terminal.
-   `incglobal`/`decglobal`: Same as above and also adjust the X server values
    so all newly started terminals will use the same fontsize.
-   `incsave`/`decsave`: Same as incglobal/decglobal and also modify the
    `~/.Xresources` file so the changed font sizes will persist over a restart
    of the X server or a reboot.
-   `reset`: Reset the font size to the value of the resource when starting the
    terminal.

You can also change the step size that the script will use to increase the font
size:

```
URxvt.font-size.step: 4
```

The default step size is 1. This means that with this setting a size change
sequence would be for example 8->12->16->20 instead of 8->9->10->11->12 etc.
Please note that many X11 fonts are only available in specific sizes, though,
and odd sizes are often not available, resulting in an effective step size of 2
instead of 1 in that case.

### Troubleshooting

At its core font-size is just a fancy wrapper around the `ESC ] 710;Pt ST` (and
711/712/713 for bold, italic, and bold-italic) escape sequences. So if you are
having trouble with this script please try the escape sequence directly like
this (outside of a terminal multiplexer like screen or tmux):

```
$ echo -e "\033]710;fixed\033\\"
```

You can exchange `fixed` for any (single) font that you want to try, like for
example `-xos4-terminus-medium-r-normal--12-120-72-72-c-60-iso10646-1`:

```
$ echo -e "\033]710;-xos4-terminus-medium-r-normal--12-120-72-72-c-60-iso10646-1\033\\"
```

If you can reproduce the problem this way then the problem does not lie with the
font-size script, but either directly with urxvt or some other component that
may interfere (like for example a window manager). Please report the bug to
those projects in that case.

fullscreen
----------
Author:  Christopher Luna  
License: GPLv3  
URL:     https://aur.archlinux.org/packages/urxvt-fullscreen

Use keyboard shortcut to toggle fullscreen.

After installing, enable the extension by adding the following line in your
`~/.Xresources`:

```
URxvt.perl-ext-common: ...,fullscreen
```

Then add a keybinding:

```
URxvt.keysym.F11: perl:fullscreen:switch
```

keyboard-select
---------------
Author:  Bert Muennich  
License: GPLv2  
URL:     http://www.github.com/muennich/urxvt-perls

Use keyboard shortcuts to select and copy text.

After installing, enable the extension by adding the following line in your
`~/.Xresources`:

```
URxvt.perl-ext-common: ...,keyboard-select
```

Then add some keybindings:

```
URxvt.keysym.M-Escape: perl:keyboard-select:activate
```

The following line overwrites the default `Meta-s` binding and allows to
activate keyboard-select directly in backward search mode:

```
URxvt.keysym.M-s: perl:keyboard-select:search
```

Use Meta-Escape to activate selection mode, then use the following keys:

-   `h`/`j`/`k`/`l`: Move cursor left/down/up/right (also with arrow keys)
-   `g`/`G`/`0`/`^`/`$`/`H`/`M`/`L`/`f`/`F`/`;`/`,`/`w`/`W`/`b`/`B`/`e`/`E`:
    More vi-like cursor movement keys
-   `/`/`?`: Start forward/backward search
-   `n`/`N`: Repeat last search, `N`: in reverse direction
-   `Ctrl-f`/`b`: Scroll down/up one screen
-   `Ctrl-d`/`u`: Scroll down/up half a screen
-   `v`/`V`/`Ctrl-v`: Toggle normal/linewise/blockwise selection
-   `y`/`Return`: Copy selection to primary buffer, `Return`: quit afterwards
-   `Y`: Copy selected lines to primary buffer or cursor line and quit
-   `q`/`Escape`: Quit keyboard selection mode

url-select
----------
Author:  Bert Muennich  
License: GPLv2  
URL: http://www.github.com/muennich/urxvt-perls

Use keyboard shortcuts to select URLs.

**DEPRECATED**

Since version 9.21 the *matcher* extension shipped with rxvt-unicode fully
replaces this extension. The `url-select:select_next` action is provided by the
`matcher:select` action. However, the *matcher* extension does not provide
vi-like key bindings; it only uses the arrow and home/end keys.

This should be used as a replacement for the default matcher extension, it also
makes URLs clickable with the middle mouse button.

After installing, enable the extension by adding the following line in your
`~/.Xresources`:

```
URxvt.perl-ext-common: ...,url-select
```

Then add some keybindings:

```
URxvt.keysym.M-u: perl:url-select:select_next
```

Use `Meta-u` to activate URL selection mode, then use the following keys:

-   `j`/`k`: Select next downward/upward URL (also with arrow keys)
-   `g`/`G`: Select first/last URL (also with `home`/`end` key)
-   `o`/`Return`: Open selected URL in browser, `Return`: deactivate afterwards
-   `y`: Copy (yank) selected URL and deactivate selection mode
-   `q`/`Escape`: Deactivate URL selection mode

Options:

-   `URxvt.url-select.autocopy`: if set to true, selected URLs are automatically
    copied to the PRIMARY buffer
-   `URxvt.url-select.button`: mouse button to click-open URLs (default: 2)
-   `URxvt.url-select.launcher`: browser/command to open selected URL with
-   `URxvt.url-select.underline`: if set to true, all URLs get underlined

For compatibility reasons, url-select will also use any patterns defined for the
matcher extension by reading all `URxvt.matcher.pattern.[0-9]` resources.

vtwheel
-------
Author:  PyroPeter  
License: Unknown  
URL: https://aur.archlinux.org/packages/urxvt-vtwheel

Scroll wheel support.

After installing, enable the extension by adding the following line in your
`~/.Xresources`:

```
URxvt.perl-ext-common: ...,vtwheel
```
