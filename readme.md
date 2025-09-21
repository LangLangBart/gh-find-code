<h1 align="center">gh find-code</h1>

<div align="center">

This extension is a command-line tool that uses the GitHub REST API and `fzf` to
interactively search and preview code.

<img
src="https://github.com/LangLangBart/gh-find-code/assets/92653266/144c966d-a5ac-4715-a7b3-7e6684bcf3d0"
alt="image.png" style="max-width: 100%; height: auto" loading="lazy"
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
> The search syntax differs between the WebUI and the REST API, with the latter
> not supporting regex.

---

| Flags | Description                                              |
| ----- | -------------------------------------------------------- |
| `-l`  | limit the number of listed results (default 30, max 100) |
| `-h`  | help                                                     |

| Key Bindings fzf | Purpose                                  | Keybind Environment Variable   |
| ---------------- | ---------------------------------------- | ------------------------------ |
| `?`              | toggle help                              |                                |
| `ctrl-b`         | open the file in the browser             | `GHFC_OPEN_BROWSER_KEY`        |
| `ctrl-o`         | open the file content in the editor      | `GHFC_OPEN_EDITOR_KEY`         |
| `ctrl-p`         | prepend "repo:{owner/name}" to the query | `GHFC_FILTER_BY_REPO_KEY`      |
| `ctrl-r`         | reload with up to 100 results            | `GHFC_RELOAD_KEY`              |
| `ctrl-space`     | toggle command history                   | `GHFC_TOGGLE_HISTORY_KEY`      |
| `ctrl-t`         | toggle between Code and Fuzzy search     | `GHFC_TOGGLE_FUZZY_SEARCH_KEY` |
| `ctrl-x`         | open the search query in the browser     | `GHFC_OPEN_BROWSER_QUERY_KEY`  |
| `enter`          | open the file in the pager               | `GHFC_VIEW_CONTENTS_KEY`       |
| `tab`            | toggle the file preview                  | `GHFC_TOGGLE_PREVIEW_KEY`      |
| `esc`            | quit                                     |                                |

To avoid interfering with a user's typical keybinds, key bindings can be customized by setting the
corresponding environment variables. For example, to change the key binding for opening a file in
the browser from `ctrl-b` to `ctrl-k`:

```sh
GHFC_OPEN_BROWSER_KEY="ctrl-k" gh find-code
```

> [!NOTE]
> The assigned key must be a valid key listed under `AVAILABLE KEYS` in the `fzf` man page.
>
> ```sh
> man fzf | less --pattern "AVAILABLE KEYS"
> ```

---

## 💻 Requirements and Installation

- [bash 4.1+](https://www.gnu.org/software/bash/) - shell that runs the script
- [bat](https://github.com/sharkdp/bat#installation) - preview looks better
- [column](https://man7.org/linux/man-pages/man1/column.1.html) - format output
- [curl](https://github.com/curl/curl) - sending updates to `fzf`
- [Fuzzy Finder (fzf)](https://github.com/junegunn/fzf#installation) - allow for
  interaction with listed data
- [GitHub command line tool (gh)](https://github.com/cli/cli#installation) - get
  the data from Github
- [GNU grep](https://www.gnu.org/software/grep/) - searches through text using regular expressions
- [Python](https://www.python.org) - used to parse and open custom URLs on
  different operating systems

```sh
# install this extension
gh ext install LangLangBart/gh-find-code
# upgrade
gh ext upgrade LangLangBart/gh-find-code
# uninstall
gh ext remove LangLangBart/gh-find-code
```

Try it in Docker (requires a `GH_TOKEN`).

```bash
docker run --rm --interactive --tty -e GH_TOKEN=<YOUR TOKEN> -e TERM alpine sh -uec '
# On Alpine, the "util-linux" package is needed for the "column" command.
apk add util-linux bash bat curl fzf github-cli grep python3
gh ext install LangLangBart/gh-find-code
gh find-code'
```

---

## 🌐 Environment Variables

### Table 1: Environment Variables Utilized

| Variable    | Purpose                                | Default            |
| ----------- | -------------------------------------- | ------------------ |
| `BAT_THEME` | Preview theme for syntax highlighting. | `Monokai Extended` |
| `EDITOR`    | Editor to open selected files.         | `vim`              |
| `PAGER`     | Pager for file viewing.                | `less`             |

### Table 2: Environment Variables Defined and Utilized

| Variable             | Purpose                       | Default                                                          |
| -------------------- | ----------------------------- | ---------------------------------------------------------------- |
| `GHFC_DEBUG_MODE`    | Enable debug mode             | `0` (Disabled)                                                   |
| `GHFC_HISTORY_FILE`  | Custom location               | `${XDG_STATE_HOME:-$HOME/.local/state}/gh-find-code/history.txt` |
| `GHFC_HISTORY_LIMIT` | Max number of stored commands | `500`                                                            |

---

## 💁 TIPS

### Alias

- The name `gh find-code` was chosen for its descriptive nature. For frequent
  use, consider setting up an alias.

```sh
# ~/.bashrc or ~/.zshrc
alias ghfc='gh find-code'
# or add a custom 'BAT_THEME'/ 'EDITOR'
alias ghfc='BAT_THEME="Dracula" EDITOR="vim" gh find-code'
```

### Bat

- Set `BAT_THEME` to change the preview color scheme:

```sh
# To view all default themes
bat --list-themes --color=never
# Recommended themes: 1337, Dracula, gruvbox-dark, Monokai Extended
# To launch this extension with the 'Dracula' theme
BAT_THEME="Dracula" gh find-code
```

### Editor

- The extension uses the `EDITOR` environment variable to determine in which
  editor the selected file will be opened, works with `nano`, `nvim/vi/vim`,
  and `VSCode` and some of its derivatives (e.g. `VSCodium`).
- The code from opened files is stored temporarily and is removed when the
  program ends.

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
- **NOTE:** [How to use ALT commands in a terminal on macOS?](https://superuser.com/questions/496090/how-to-use-alt-commands-in-a-terminal-on-os-x)

### History

- The history file stores successfully completed unique commands.
- Customize history file location and limit:

```sh
# Specify a custom location for the history file
GHFC_HISTORY_FILE="/custom/location/history.txt" gh find-code
# Set the maximum number of stored commands to 1000
GHFC_HISTORY_LIMIT="1000" gh find-code
```

### Pattern Matching

- In rare cases, when the API returns patterns with newline characters, `pcre2grep`, `pcregrep`, or
  `rg` will be used to find line numbers if any of them is installed. Otherwise, `grep` will be used
  by default, which will not match patterns containing newlines.

---

## 🤔 Pitfall

### Incorrect Line Numbers

The API may return irrelevant `text` matches, which can lead to incorrect line numbers in the entire
document.

I occasionally encounter this issue in `readme.md` files when the fragment contains Asian characters
prior to the desired search keyword. For example, when searching for `commander.js` in the
`nieweidong/fetool` repository, the API may return text matches that are all 12 characters long, but
there seems to be a bug in the GitHub code as it does not correctly count when the search keyword
appears in the fragment. Instead of matching `commander.js`, it returns `er](https://`.

- Ref: [GitHub Docs - Text Match Metadata](https://docs.github.com/en/rest/search/search#text-match-metadata)

```sh
command gh api search/code --method GET --cache 1h --field per_page=1 \
  --header 'Accept: application/vnd.github.text-match+json' \
  --raw-field 'q=repo:nieweidong/fetool commander.js' \
  --jq '.items[].text_matches[].matches | first | {text}'
```

```json
{
  "text": "er](https://"
}
```

Here is an example of a proper `text` response from the search API:

```sh
command gh api search/code --method GET --cache 1h --field per_page=1 \
  --header 'Accept: application/vnd.github.text-match+json' \
  --raw-field 'q=repo:calvinmetcalf/ltcdr commander.js' \
  --jq '.items[].text_matches[].matches | first | {text}'
```

```json
{
  "text": "commander.js"
}
```

---

## 💪 Contributing

> [!NOTE]
> _Pre-commit is a multi-language package manager for pre-commit hooks. You_
> _specify a list of hooks you want and pre-commit **manages** the installation_
> _and **execution** of any hook written in any language before every commit._
>
> **Source:** [pre-commit introduction](https://pre-commit.com/#introduction)

```sh
# install the git hook scripts
pre-commit install --hook-type commit-msg --hook-type pre-commit
# pre-commit installed at .git/hooks/commit-msg
# pre-commit installed at .git/hooks/pre-commit
```

---

## ⭐ Noteworthy Projects

- [Official GitHub Search](https://github.com/search?type=code)
- [grep.app | code search](https://grep.app/)
- [k1LoW/gh-grep](https://github.com/k1LoW/gh-grep)
- [johnlindquist/ghx](https://github.com/johnlindquist/ghx)
