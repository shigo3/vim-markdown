# Vim Markdown

[![Build Status](https://travis-ci.org/rcmdnk/vim-markdown.svg?branch=mod)](https://travis-ci.org/rcmdnk/vim-markdown)

Syntax highlighting, matching rules and mappings for [the original Markdown](http://daringfireball.net/projects/markdown/) and extensions.

- - -

**rcmdnk's modified version**

* [Syntax hilight test with rcmdnk version](https://rcmdnk.com/vim-markdown/syntax-test.html)

* Changed filetype name `mkd` to `markdown`. (:information_source: plasticboy version has also changed to use `markdown` since 9/Sep/2015.)
* ftdetect/markdown.vim
    * Added tmp, old: {md,...}.{...tmp,old}
* syntax/markdown.vim
    * Enabled spell check at most of place.
    * Enable html syntax.
    * `(URL)` is enabled only after `[LINK]`.
    * Enabled mkdInlineURL (modified).
    * Delimiters for codes are set as mkdDelimiter.
    * Added liquidTag (`{%~%}`)
        * `let g:vim_markdown_liquid=0` to disable liquidTag (default: enabled).
    * Added PHP Markdown Extra codeblock (`~~~`)
    * <strike>Added syntax in codeblock inspired by [vim-markdown-quote-syntax](https://github.com/joker1007/vim-markdown-quote-syntax)</strike>
        * <strike>You can disable this feature by `let g:vim_markdown_codeblock_syntax=0`.</strike>
        * <strike>Use forked version of [rcmdnk/vim-markdown-quote-syntax](https://github.com/rcmdnk/vim-markdown-quote-syntax)
        for codeblock syntaxes</strike>
        * Original [vim-markdown-quote-syntax](https://github.com/joker1007/vim-markdown-quote-syntax)
        has now these features.
* Better folding
    * `let g:vim_markdown_better_folding=1` allows better folding, especially
    for YAML block, Jekyll/Octopress codeblock.
    * Note: It may make it slow for large files.

If you use [Vundle](https://github.com/gmarik/Vundle.vim), do:

```vim
Plugin 'godlygeek/tabular'
Plugin 'joker1007/vim-markdown-quote-syntax'
Plugin 'rcmdnk/vim-markdown'
```

or with [Dein.vim](https://github.com/Shougo/dein.vim):

```vim
call dein#add('godlygeek/tabular')
call dein#add('joker1007/vim-markdown-quote-syntax')
call dein#add('rcmdnk/vim-markdown')
```
- - -

1. [Installation](#installation)
1. [Options](#options)
1. [Mappings](#mappings)
1. [Commands](#commands)
1. [Credits](#credits)
1. [License](#license)

## Installation

If you use [Vundle](https://github.com/gmarik/vundle), add the following line to your `~/.vimrc`:

```vim
Plugin 'godlygeek/tabular'
Plugin 'plasticboy/vim-markdown'
```

The `tabular` plugin must come *before* `vim-markdown`.

Then run inside Vim:

```vim
:so ~/.vimrc
:PluginInstall
```

If you use [Pathogen](https://github.com/tpope/vim-pathogen), do this:

```sh
cd ~/.vim/bundle
git clone https://github.com/plasticboy/vim-markdown.git
```

To install without Pathogen using the Debian [vim-addon-manager](http://packages.qa.debian.org/v/vim-addon-manager.html), do this:

```sh
git clone https://github.com/plasticboy/vim-markdown.git
cd vim-markdown
sudo make install
vim-addon-manager install markdown
```

If you are not using any package manager, download the [tarball](https://github.com/plasticboy/vim-markdown/archive/master.tar.gz) and do this:

```sh
cd ~/.vim
tar --strip=1 -zxf vim-markdown-master.tar.gz
```

## Options

### Disable Folding

Add the following line to your `.vimrc` to disable the folding configuration:

```vim
let g:vim_markdown_folding_disabled = 1
```

This option only controls Vim Markdown specific folding configuration.

To enable/disable folding use Vim's standard folding configuration.

```vim
set [no]foldenable
```

### Set header folding level

Folding level is a number between 1 and 6. By default, if not specified, it is set to 1.

```vim
let g:vim_markdown_folding_level = 6
```

Tip: it can be changed on the fly with:

```vim
:let g:vim_markdown_folding_level = 1
:edit
```

### Disable Default Key Mappings

Add the following line to your `.vimrc` to disable default key mappings:

```vim
let g:vim_markdown_no_default_key_mappings = 1
```

You can also map them by yourself with `<Plug>` mappings.

### Enable TOC window auto-fit

Allow for the TOC window to auto-fit when it's possible for it to shrink.
It never increases its default size (half screen), it only shrinks.

```vim
let g:vim_markdown_toc_autofit = 1
```

### Text emphasis restriction to single-lines

By default text emphasis works across multiple lines until a closing token is found. However, it's possible to restrict text emphasis to a single line (ie, for it to be applied a closing token must be found on the same line). To do so:

```vim
let g:vim_markdown_emphasis_multiline = 0
```

### Syntax Concealing

Concealing is set for some syntax.

For example, conceal `[link text](link url)` as just `link text`.
Also, `_italic_` and `*italic*` will conceal to just _italic_.
Similarly `__bold__`, `**bold**`, `___italic bold___`, and `***italic bold***`
will conceal to just __bold__, **bold**, ___italic bold___, and ***italic bold*** respectively.

To enable conceal use Vim's standard conceal configuration.

```vim
set conceallevel=2
```

To disable conceal regardless of `conceallevel` setting, add the following to your `.vimrc`:

```vim
let g:vim_markdown_conceal = 0
```

To disable math conceal with LaTeX math syntax enabled, add the following to your `.vimrc`:

```vim
let g:tex_conceal = ""
let g:vim_markdown_math = 1
```

### Syntax extensions

The following options control which syntax extensions will be turned on. They are off by default.

#### LaTeX math

Used as `$x^2$`, `$$x^2$$`, escapable as `\$x\$` and `\$\$x\$\$`.

```vim
let g:vim_markdown_math = 1
```

#### YAML Front Matter

Highlight YAML front matter as used by Jekyll or [Hugo](https://gohugo.io/content/front-matter/).

```vim
let g:vim_markdown_frontmatter = 1
```

#### TOML Front Matter

Highlight TOML front matter as used by [Hugo](https://gohugo.io/content/front-matter/).

TOML syntax highlight requires [vim-toml](https://github.com/cespare/vim-toml).

```vim
let g:vim_markdown_toml_frontmatter = 1
```

#### JSON Front Matter

Highlight JSON front matter as used by [Hugo](https://gohugo.io/content/front-matter/).

JSON syntax highlight requires [vim-json](https://github.com/elzr/vim-json).

```vim
let g:vim_markdown_json_frontmatter = 1
```

### Do not require .md extensions for Markdown links

If you want to have a link like this `[link text](link-url)` and follow it for editing in vim using the `ge` command, but have it open the file "link-url.md" instead of the file "link-url", then use this option:

```vim
let g:vim_markdown_no_extensions_in_markdown = 1
```
This is super useful for GitLab and GitHub wiki repositories.

Normal behaviour would be that vim-markup required you to do this `[link text](link-url.md)`, but this is not how the Gitlab and GitHub wiki repositories work. So this option adds some consistency between the two. 

### Auto-write when following link

If you follow a link like this `[link text](link-url)` using the `ge` shortcut, this option will automatically save any edits you made before moving you:

```vim
let g:vim_markdown_autowrite = 1
```

## Mappings

The following work on normal and visual modes:

-   `gx`: open the link under the cursor in the same browser as the standard `gx` command. `<Plug>Markdown_OpenUrlUnderCursor`

    The standard `gx` is extended by allowing you to put your cursor anywhere inside a link.

    For example, all the following cursor positions will work:

        [Example](http://example.com)
        ^  ^    ^^   ^       ^
        1  2    34   5       6

        <http://example.com>
        ^  ^               ^
        1  2               3

    Known limitation: does not work for links that span multiple lines.

-   `ge`: open the link under the cursor in Vim for editing. Useful for relative markdown links. `<Plug>Markdown_EditUrlUnderCursor`

    The rules for the cursor position are the same as the `gx` command.

-   `]]`: go to next header. `<Plug>Markdown_MoveToNextHeader`

-   `[[`: go to previous header. Contrast with `]c`. `<Plug>Markdown_MoveToPreviousHeader`

-   `][`: go to next sibling header if any. `<Plug>Markdown_MoveToNextSiblingHeader`

-   `[]`: go to previous sibling header if any. `<Plug>Markdown_MoveToPreviousSiblingHeader`

-   `]c`: go to Current header. `<Plug>Markdown_MoveToCurHeader`

-   `]u`: go to parent header (Up). `<Plug>Markdown_MoveToParentHeader`

This plugin follows the recommended Vim plugin mapping interface, so to change the map `]u` to `asdf`, add to your `.vimrc`:

    map asdf <Plug>Markdown_MoveToParentHeader

To disable a map use:

    map <Plug> <Plug>Markdown_MoveToParentHeader

## Commands

The following requires `:filetype plugin on`.

-   `:HeaderDecrease`:

    Decrease level of all headers in buffer: `h2` to `h1`, `h3` to `h2`, etc.

    If range is given, only operate in the range.

    If an `h1` would be decreased, abort.

    For simplicity of implementation, Setex headers are converted to Atx.

-   `:HeaderIncrease`: Analogous to `:HeaderDecrease`, but increase levels instead.

-   `:SetexToAtx`:

    Convert all Setex style headers in buffer to Atx.

    If a range is given, e.g. hit `:` from visual mode, only operate on the range.

-   `:TableFormat`: Format the table under the cursor [like this](http://www.cirosantilli.com/markdown-style-guide/#tables).

    Requires [Tabular](https://github.com/godlygeek/tabular).

    The input table *must* already have a separator line as the second line of the table.
    That line only needs to contain the correct pipes `|`, nothing else is required.

-   `:Toc`: create a quickfix vertical window navigable table of contents with the headers.

    Hit `<Enter>` on a line to jump to the corresponding line of the markdown file.

-   `:Toch`: Same as `:Toc` but in an horizontal window.

-   `:Toct`: Same as `:Toc` but in a new tab.

-   `:Tocv`: Same as `:Toc` for symmetry with `:Toch` and `:Tocv`.

## Credits

The main contributors of vim-markdown are:

- **Ben Williams** (A.K.A. **plasticboy**). The original developer of vim-markdown. [Homepage](http://plasticboy.com/).

If you feel that your name should be on this list, please make a pull request listing your contributions.

## License

The MIT License (MIT)

Copyright (c) 2012 Benjamin D. Williams

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
