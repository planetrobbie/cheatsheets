{
    "id": "auto",
    "style": "css/h4dark.css",
    "layout": "default"
}
%%%END

# Sublime Text

#### General

command | mac | linux/win
-- | -- | --
Open Command Palette | ⌘ + ⇧ + P | Ctrl + ⇧ + P
Toggle Side Bar | ⌘ + K, ⌘ + B | Ctrl + KB
Show Scope (Status Bar) | ⌃ + ⇧ + P | Ctrl + ⇧ + Alt + P
Toggle Fullscreen | ⌃ + ⌘ + F |
Python Console | ⌃ + ` |

#### Editing

command | mac | linux/win
-- | -- | --
Delete Line | ⌘ + X | Ctrl + X
Insert Line After | ⌘ + ↩ | Ctrl + ↩
Insert Line Before | ⌘ + ⇧ + ↩ | Ctrl + ⇧ + ↩
Move Line Up | ⌘ + ⌃ + ↑ | Ctrl + ⇧ + ↑
Move Line Down | ⌘ + ⌃ + ↓ | Ctrl + ⇧ + ↓
Select Line | ⌘ + L | Ctrl + L
Select Word | ⌘ + D | Ctrl + D
Multi-Select Editing | ⌃ + ⌘ + G | Alt + F3
Jump to Closing Parenthesis | ⌃ + M | Ctrl + M
Select Contents of Current Parentheses | ⌃ + ⇧ + M | Ctrl + ⇧ + M
Delete from Cursor to End of Line | ⌘ + K,⌘ + K | Ctrl + KK
Delete from Cursor to Start of Line | ⌘ + K + ⌫ | Ctrl + K + ⌫
Indent Current Line(s) | ⌘ + ] | Ctrl + ]
Unindent Current Line(s) | ⌘ + [ | Ctrl + [
Duplicate Line(s) | ⌘ + ⇧ + D | Ctrl + ⇧ + D
Join Line Below Current Line | ⌘ + J | Ctrl + J
Comment/Uncomment Current Line | ⌘ + / | Ctrl + /
Block Comment Current Selection | ⌘ + ⌥ + / | Ctrl + ⇧ + /
Redo/Repeat Last Keyboard Shortcut | ⌘ + Y | Ctrl + Y
Paste with Proper Indenting | ⌘ + ⇧ + V | Ctrl + ⇧ + V
Select Next Autocomplete Suggestion | ⌃ + Space | Ctrl + Space
Soft Undo | ⌃ + U | Ctrl + U

#### Transform

command | mac | linux/win
-- | -- | --
To Uppercase | ⌘ + K, ⌘ + U | Ctrl + KU
To lowercase | ⌘ + K, ⌘ + L | Ctrl + KL
Increase Font Size | ⌘ + | Ctrl +
Decrease Font Size | ⌘ - | Ctrl -

#### Find &amp; Replace

command | mac | linux/win
-- | -- | --
Find | ⌘ + F | Ctrl + F
Replace | ⌘ + ⌥ + F | Ctrl + H
Find in Files | ⌘ + ⇧ + F | Ctrl + ⇧ + F
Use Selection for Find | ⌘ + E | Ctrl + E
Use Selection for Replace | ⇧ + ⌘ + E | ⇧ + Ctrl + E

#### Navigation

command | mac | linux/win
-- | -- | --
Quick Open File (by Name) | ⌘ + P | Ctrl + P
Go To Symbol | ⌘ + R | Ctrl + R
Go To Line | ⌃ + G | Ctrl + G
Go To Start Of File | ⌘ + Up | Ctrl + Up
Go To End Of File | ⌘ + Down | Ctrl + Down
Jump to tab 1-9 | ⌘ + [NUM] | Alt + [NUM]
Cycle up through recent tabs | ^ + Tab |
Cycle right through tabs | ⌘ + ⇧ + ] | Ctrl + PgUp
Cycle left through tabs | ⌘ + ⇧ + [ | Ctrl + PgDn
Open last closed tab | ⌘ + ⇧ + t | Ctrl + ⇧ + t

#### Code Folding

command | mac | linux/win
-- | -- | --
Fold Selection | ⌥ + ⌘ + [ | ⇧ + Ctrl + [
Unfold Selection | ⌥ + ⌘ + ] | ⇧ + Ctrl + ]
Unfold All | ⌘ + K, ⌘ + J | Ctrl +KJ

#### Bookmarks

command | mac | linux/win
-- | -- | --
Toggle Bookmark | ⌘ + F2 | Ctrl + F2
Next Bookmark | F2 | F2
Previous Bookmark | ⇧ + F2 | ⇧ + F2
Clear Bookmarks | ⇧ + ⌘ + F2 | ⇧ + Ctrl + F2

#### Split

command | mac | linux/win
-- | -- | --
Revert view to single column | ⌘ + ⌥ + 1 | Alt + ⇧ + 1
Split view into two columns | ⌘ + ⌥ + 2 | Alt + ⇧ + 2
Split view into three columns | ⌘ + ⌥ + 3 | Alt + ⇧ + 3
Split view into four columns | ⌘ + ⌥ + 4 | Alt + ⇧ + 4
Set view to grid (4 groups) |  ⌘ + ⌥ + 5 | Alt + ⇧ + 5
Jump to group where num is 1-4 | ⌃ + [NUM] | Ctrl + [NUM]
Move file to specified group where num is 1-4 | ⌃ + ⇧ + [NUM] | Ctrl + ⇧ + [NUM]

#### Language

command | mac | linux/win
-- | -- | --
Select File Language | ⌘ + ⇧ + P | Ctrl + ⇧ + P

Then use the `Set Syntax: [language]` selection that is automatically populated in the command menu when you type the programming language.

#### Snippets

command | mac/linux/win
-- | --
Create a New Snippet. | Tools &gt; Developer &gt; New Snippet...

On mac, snippets are stored in `~/Library/Application\ Support/Sublime\ Text\ 3/Packages/User/`.
See [documentation](http://docs.sublimetext.info/en/latest/extensibility/snippets.html) for further details.

#### Markdown Lint

command | mac | linux/win
-- | -- | --
Linting Markdown | ⌘ + ⇧ + P &gt; MarkdownEditing Lint | Ctrl + ⇧ + P &gt; MarkdownEditing Lint

Require `SublimeLinter` and `MardownEditing` packages.

#### Interesting Packages

- [SublimeLinter](https://packagecontrol.io/search/Markdown%20lint) - npm required
- [Markdown​Editing](https://packagecontrol.io/packages/MarkdownEditing) - eye candy theme !
- [GoSublime](https://packagecontrol.io/packages/GoSublime)
- [Mediawikier](https://packagecontrol.io/packages/Mediawiker)
- [RegReplace](https://packagecontrol.io/packages/RegReplace)
---

Cheatsheet freely available on [GitHub](source available on [GitHub](https://github.com/planetrobbie/cheatsheets/sublime.md), adapter to [Cheaters](http://brettterpstra.com/projects/cheaters) by planetrobbie from [Chris Simpkins](http://sweetme.at/2013/08/08/sublime-text-keyboard-shortcuts/) version.
**Sublime Text** is available at [https://www.sublimetext.com/](https://www.sublimetext.com/)