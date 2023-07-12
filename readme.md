<div align="center">

# gh find-code

This extension is a command-line tool that uses the GitHub REST API and `fzf` to
interactively search and preview code.

<img
src="https://github.com/LangLangBart/gh-find-code/assets/92653266/144c966d-a5ac-4715-a7b3-7e6684bcf3d0"
width="800">

</div>

---

## 👨‍💻 Usage

```sh
gh find-code [Flags] [Search term]
```

```sh
# matches code from @junegunn's 'fzf' repo
gh find-code 'repo:junegunn/fzf FZF_PORT'
# matches JavaScript files with "new Proxy()"
gh find-code 'language:js "new Proxy()"'
```

| Flags | Description                                              | Example                                      |
| ----- | -------------------------------------------------------- | -------------------------------------------- |
| `-d`  | debug mode, temporary files are not deleted on exit      | `gh find-code -d`                            |
| `-l`  | limit the number of listed results (default 30, max 100) | `gh find-code -l 50 'filename:_fzf compdef'` |
| `-h`  | help                                                     | `gh find-code -h`                            |

| Key Bindings fzf            | Description                                               |
| --------------------------- | --------------------------------------------------------- |
| <kbd>?</kbd>                | help                                                      |
| <kbd>;</kbd>                | quick jump                                                |
| <kbd>ctrl</kbd><kbd>b</kbd> | open the file in the browser                              |
| <kbd>ctrl</kbd><kbd>e</kbd> | open the file content in an editor, works with VSCode/Vim |
| <kbd>ctrl</kbd><kbd>o</kbd> | open the search query in the browser                      |
| <kbd>ctrl</kbd><kbd>u</kbd> | clear the query                                           |
| <kbd>enter</kbd>            | open the file with the pager                              |
| <kbd>tab</kbd>              | toggle the file preview                                   |
| <kbd>esc</kbd>              | quit                                                      |

---

## 💻 Requirements
- [bat](https://github.com/sharkdp/bat#installation) - preview looks better
- [Fuzzy Finder (fzf)](https://github.com/junegunn/fzf#installation) - allow for
  interaction with listed data
- [GitHub command line tool (gh)](https://github.com/cli/cli#installation) - get the data
  from Github
- [Python](https://www.python.org) - used to parse and open custom URLs on different
  operating systems

```sh
# install requirements
brew install bat fzf gh python

# install this extension
gh ext install LangLangBart/gh-find-code
# upgrade
gh ext upgrade LangLangBart/gh-find-code
# uninstall
gh ext remove LangLangBart/gh-find-code
```

---

## 💁 TIPS

### Search syntax
- GitHub REST API is used to search for code. The correct query syntax for searching code
  is detailed in the links below.
  - [GitHub Docs - Searching Code](https://docs.github.com/en/search-github/searching-on-github/searching-code)
  - [GitHub REST API - search](https://docs.github.com/en/rest/search/search#search-code)

<sub>⚠️ The search syntax differs between the WebUI and the REST API, with the latter not
supporting regex.</sub>

### Alias
- The name `gh find-code` was chosen for its descriptive nature. For frequent use,
  consider setting up an alias.

```sh
# ~/.bashrc or ~/.zshrc
alias ghfc='gh find-code'
# or add a custom 'BAT_THEME'/ 'EDITOR'
alias ghfc='BAT_THEME="Dracula" EDITOR="vim" gh find-code'
```

### Bat
- The color scheme of the preview is determined by the `BAT_THEME` environment variable.
  If not explicitly set by the user, the theme defaults to `Monokai Extended`.

```sh
# To view all default themes
bat --list-themes --color=never
# Recommended themes: 1337, Dracula, gruvbox-dark, Monokai Extended
# To launch this extension with the 'Dracula' theme
BAT_THEME="Dracula" gh find-code
```

### Editor
- The extension uses the `EDITOR` environment variable to open files in your editor.
  Currently, only `VSCode` and `Vim` are supported. If the `EDITOR` variable isn't set,
  nothing will happen. You can specify your preferred editor when launching the extension.
- The code from these files is held temporarily and gets removed when this program ends.

```sh
# Set the editor to Visual Studio Code
EDITOR="code" gh find-code
```

### Fuzzy Finder (fzf)
- Scroll the preview in larger steps by adding this snippet to your shell setup.

```sh
# ~/.bashrc or ~/.zshrc
# scroll the preview in larger steps with ctrl+w/s
export FZF_DEFAULT_OPTS="
--bind 'ctrl-w:preview-half-page-up,ctrl-s:preview-half-page-down'"
```

- See `man fzf` for `AVAILABLE KEYS` or
  [junegunn/fzf#environment-variables](https://github.com/junegunn/fzf#environment-variables)
  on GitHub for more details.
- NOTE: [How to use ALT commands in a terminal on
  macOS?](https://superuser.com/questions/496090/how-to-use-alt-commands-in-a-terminal-on-os-x)

### Good to know
- Fast movements between list items can cause the wrong file to be displayed in the
  preview.
- Be careful which files you open in your editor to avoid triggering something unintended.

### Pager
- If the `PAGER` environment variable is set to `less`, when opening the destination file, it
  will automatically scroll to the matching line found.

---

## 💪 Contributing
- The [pre-commit](https://github.com/pre-commit/pre-commit) tool is used to handle
  routine code checks. To customize the tool, refer to the
  [.pre-commit-config.yaml](.pre-commit-config.yaml) file.

> Pre-commit is a multi-language package manager for pre-commit hooks. You specify a list
> of hooks you want and **pre-commit manages the installation and execution** of any hook
> written in any language before every commit. Source: [pre-commit
> introduction](https://pre-commit.com/#introduction)

```sh
# install
brew pre-commit

# install the git hook scripts
pre-commit install --hook-type commit-msg --hook-type pre-commit
# pre-commit installed at .git/hooks/commit-msg
# pre-commit installed at .git/hooks/pre-commit

# hook location
.git/hooks/pre-commit
.git/hooks/commit-msg
```
