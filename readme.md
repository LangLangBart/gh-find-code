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
gh find-code [Flags] [Search query]
```

- Use valid qualifiers to refine the results of your search.
  - [GitHub Docs - Searching Code](https://docs.github.com/en/search-github/searching-on-github/searching-code)
  - [GitHub Docs - Understanding the search syntax](https://docs.github.com/en/search-github/getting-started-with-searching-on-github/understanding-the-search-syntax)

| Qualifier                      | Search query example             | Description                                             |
| ------------------------------ | -------------------------------- | ------------------------------------------------------- |
| `in`                           | `'in:path zsh'`                  | matches code where `zsh` appears in the file path       |
| `user`                         | `'user:ashtom Development'`      | files with the word `Development` only from `@ashtom`   |
| `org`                          | `'org:cli searcher'`             | searches all code in the `cli` org for `searcher`       |
| `repo`                         | `'repo:junegunn/fzf FZF_PORT'`   | searches only in the `junegunn/fzf` repo for `FZF_PORT` |
| `path`                         | `'path:.github shfmt'`           | files with the word `shfmt` in the `.github` path       |
| `language`                     | `'language:js "new Proxy"'`      | search for the string `new Proxy` in JavaScript files   |
| `size` <br> (>, >=, <, and <=) | `'size:<100 _gnu_generic'`       | files smaller than 100 bytes with `_gnu_generic`        |
| `filename`                     | `'filename:.zshrc GOCACHE'`      | search in all filenames `.zshrc` for `GOCACHE`          |
| `extension`                    | `'extension:rs "Hello, world!"'` | find `.rs` files with the string `Hello, world!`        |


> [!IMPORTANT]
> The search syntax differs between the WebUI and the REST API, with the latter not
> supporting regex.

---

| Flags | Description                                                   |
| ----- | ------------------------------------------------------------- |
| `-d`  | debug mode, primarily used for identifying and resolving bugs |
| `-l`  | limit the number of listed results (default 30, max 100)      |
| `-h`  | help                                                          |

| Key Bindings fzf            | Description                              |
| --------------------------- | ---------------------------------------- |
| <kbd>?</kbd>                | help                                     |
| <kbd>ctrl</kbd><kbd>b</kbd> | open the file in the browser             |
| <kbd>ctrl</kbd><kbd>h</kbd> | display the history commands             |
| <kbd>ctrl</kbd><kbd>o</kbd> | open the file content in the editor      |
| <kbd>ctrl</kbd><kbd>p</kbd> | replace query with "repo:<owner>/<name>" |
| <kbd>ctrl</kbd><kbd>r</kbd> | reload with up to 100 results            |
| <kbd>ctrl</kbd><kbd>t</kbd> | toggle between Code and Fuzzy search     |
| <kbd>ctrl</kbd><kbd>x</kbd> | open the search query in the browser     |
| <kbd>enter</kbd>            | open the file in the pager               |
| <kbd>tab</kbd>              | toggle the file preview                  |
| <kbd>esc</kbd>              | quit                                     |

---

## 💻 Requirements and Installation
- [bat](https://github.com/sharkdp/bat#installation) - preview looks better
- [curl](https://github.com/curl/curl) - sending updates to `fzf`
- [Fuzzy Finder (fzf)](https://github.com/junegunn/fzf#installation) - allow for
  interaction with listed data
- [GitHub command line tool (gh)](https://github.com/cli/cli#installation) - get the data
  from Github
- [Python](https://www.python.org) - used to parse and open custom URLs on different
  operating systems

```sh
# install this extension
gh ext install LangLangBart/gh-find-code
# upgrade
gh ext upgrade LangLangBart/gh-find-code
# uninstall
gh ext remove LangLangBart/gh-find-code
```

---

## 💁 TIPS

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
- The extension uses the `EDITOR` environment variable to determine in which editor
  the selected file will be opened, works with `nano`, `nvim/vi/vim`,  and
  `VSCode/VSCodium`.
- The code from opened files is stored temporarily and is removed when the program ends.

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
  [junegunn/fzf](https://github.com/junegunn/fzf#environment-variables) for more
  details.
- NOTE: [How to use ALT commands in a terminal on
  macOS?](https://superuser.com/questions/496090/how-to-use-alt-commands-in-a-terminal-on-os-x)

### History
- The `gh_find_code_history.txt` file stores successfully completed unique commands. All commands
  can be viewed with <kbd>⌃ Control</kbd> + <kbd>H</kbd>. In case of duplicates, only the most
  recent entry is preserved. The maximum number of command entries is 250 by default, but this can
  be overridden by assigning a value to the `MAX_LINES_HISTORY` variable.

```sh
# Set the maximum number of stored commands to 1000
MAX_LINES_HISTORY="1000" gh find-code
```

### Pager
- If the `PAGER` environment variable is set to `less` or `bat`, when opening the destination file,
  it will automatically scroll to the matching line found.

---

## 💪 Contributing

> [!NOTE]
> _Pre-commit is a multi-language package manager for pre-commit hooks. You specify a list_
> _of hooks you want and **pre-commit manages the installation and execution** of any hook_
> _written in any language before every commit._ **Source:** [pre-commit
> introduction](https://pre-commit.com/#introduction)

```sh
brew install pre-commit

# install the git hook scripts
pre-commit install --hook-type commit-msg --hook-type pre-commit
# pre-commit installed at .git/hooks/commit-msg
# pre-commit installed at .git/hooks/pre-commit
```
