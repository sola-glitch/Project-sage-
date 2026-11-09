# Editor Setup Guide: VS Code and other editors

This guide takes you from a fresh download to running Sage programs in your editor. Parts 1-7
cover Visual Studio Code (the best-supported editor); part 8 covers Vim, Neovim, Emacs,
Sublime Text, JetBrains IDEs, Notepad++, nano and others.

This part takes you from a fresh download to running Sage programs with one click in
Visual Studio Code. It has four steps: build the interpreter, put it on your PATH,
install the extension (the `.vsix` file), and run a program.

The extension gives you syntax highlighting, smart indentation, snippets, and run
buttons. It does **not** include a debugger, auto-complete from your code, or red
squiggles for errors. Errors appear in the terminal.

> The extension package follows the standard `.vsix` layout and its files were checked
> (valid JSON, valid JavaScript, every highlighting pattern compiles), but it has not been
> installed in a real copy of VS Code yet. If something does not work, see
> [Troubleshooting](#troubleshooting) and tell me what you see.

---

## 1. What you need

| Item | Why | Check it |
|---|---|---|
| VS Code 1.70 or newer | the editor | `code --version` |
| A C compiler and `make` | to build the Sage interpreter | `gcc --version`, `make --version` |

### Getting a C compiler

- **Linux (Ubuntu/Debian):** `sudo apt install build-essential`
- **macOS:** `xcode-select --install`
- **Windows:** the easiest route is **WSL** (Windows Subsystem for Linux). In PowerShell run
  `wsl --install`, restart, then use the Ubuntu steps above inside WSL. Sage was built and
  tested on Linux; I have not tested a native Windows build.
- **Android (Termux):** `pkg install clang make`

---

## 2. Build the interpreter

Unzip the Sage project, then in a terminal inside the `sage` folder:

```
make
make test        # optional: should say "11 passed, 0 failed"
```

This creates the `sage` program in the same folder. Try it:

```
./sage -e 'say "Hello from Sage"'
```

### Put `sage` on your PATH

Pick one:

**A. Install system-wide (Linux/macOS/WSL):**

```
sudo make install            # copies to /usr/local/bin/sage
```

No admin rights? Install into your home folder instead:

```
make install PREFIX=$HOME/.local
# make sure ~/.local/bin is on PATH (add to ~/.bashrc or ~/.zshrc):
export PATH="$HOME/.local/bin:$PATH"
```

**B. Do not touch PATH.** Leave `sage` where it is and give VS Code the full path in the
`sage.executablePath` setting (step 4).

Check: open a **new** terminal and run `sage --version`. You should see `Sage 1.0.0.0`.

---

## 3. Install the extension

The extension file is `vscode/sage-lang-1.0.0.vsix` inside the project.

### Option A: from the VS Code window

1. Open the **Extensions** view (`Ctrl+Shift+X`, or `Cmd+Shift+X` on macOS).
2. Click the **`...`** menu at the top right of that panel.
3. Choose **Install from VSIX...**
4. Pick `sage-lang-1.0.0.vsix`.
5. Click **Reload** if VS Code asks.

### Option B: from a terminal

```
code --install-extension vscode/sage-lang-1.0.0.vsix
```

(If `code` is not found: in VS Code press `Ctrl+Shift+P` and run
**Shell Command: Install 'code' command in PATH**.)

### Using WSL or a remote machine

Open your folder with the **WSL** extension (`code .` from inside WSL). Then install the
`.vsix` while connected, so it lands in the "WSL: Ubuntu" section of the Extensions view.
Because Sage lives inside WSL, this is what you want.

### Check it worked

Create a file called `hello.sage`. The bottom-right corner of VS Code should say **Sage**,
and keywords should be coloured. If it says "Plain Text", click it and choose **Sage**.

---

## 4. Point VS Code at your interpreter

If `sage --version` works in a fresh terminal you can skip this.

Otherwise open **Settings** (`Ctrl+,`), search for `sage`, and set
**Sage: Executable Path** to the full path, for example:

```
/home/you/sage/sage
```

or in `settings.json`:

```json
{
  "sage.executablePath": "/home/you/sage/sage"
}
```

---

## 5. Run your first program

1. Make a folder for your code and open it in VS Code (**File > Open Folder**).
2. Create `hello.sage`:

   ```
   let name = "world"
   say f"Hello, {name}!"

   class Greeter:
       define init(who):
           self.who = who
       define greet():
           say "Nice to meet you,", self.who

   Greeter("Sage").greet()
   ```

3. Press the **play button** at the top right of the editor, or `Ctrl+Alt+R`
   (`Cmd+Alt+R` on macOS).

The file is saved automatically and the output appears in a terminal named **Sage**.

### Commands and shortcuts

| What | How |
|---|---|
| Run the file | play button, `Ctrl+Alt+R`, or Command Palette: **Sage: Run File** |
| Run the selected lines (or the current line) | `Ctrl+Alt+Enter`, or right-click > **Sage: Run Selection** |
| Open an interactive prompt | Command Palette (`Ctrl+Shift+P`): **Sage: Open REPL** |

"Run Selection" runs the selected text as its own small program. It does not remember
earlier selections; use the REPL if you want to build things up line by line.

### Snippets

Type the prefix and press `Tab`:

| Prefix | Gives you |
|---|---|
| `say`, `let` | print / variable |
| `if`, `ifelse`, `elseif` | conditionals |
| `for`, `forrange`, `while`, `repeat` | loops |
| `define` (or `def`), `kwargs`, `lambda` | functions, with `*args` / keyword options |
| `global`, `nonlocal`, `unpack` | scope declarations, unpacking |
| `class`, `classextends`, `totext` | classes, subclass, `to_text` |
| `try`, `tryfinally`, `raise` | error handling (`catch Kind as e`, `finally`) |
| `with`, `decorator`, `import`, `from` | context manager, decorator, modules |
| `comp`, `pipe`, `fstr`, `use` | comprehension, pipeline, f-string, include |

---

## 6. Optional: a build-and-run task

If you prefer VS Code's task system, create `.vscode/tasks.json` in your project:

```json
{
  "version": "2.0.0",
  "tasks": [
    {
      "label": "Run Sage file",
      "type": "shell",
      "command": "sage",
      "args": ["${file}"],
      "group": { "kind": "build", "isDefault": true },
      "presentation": { "reveal": "always", "panel": "shared", "clear": true }
    }
  ]
}
```

Now `Ctrl+Shift+B` runs the file you are editing.

Useful editor settings for Sage (in `settings.json`):

```json
{
  "[sage]": {
    "editor.tabSize": 4,
    "editor.insertSpaces": true,
    "editor.detectIndentation": false,
    "editor.rulers": [80]
  }
}
```

Sage blocks are defined by indentation, so keep spaces (not a mix of tabs and spaces).

---

## 7. Rebuilding or customizing the extension

Edit the files in `vscode/` (the grammar is `syntaxes/sage.tmLanguage.json`, snippets are in
`snippets/sage.json`), then rebuild:

```
cd vscode
python3 build_vsix.py
```

This needs only Python 3 (no npm or `vsce`). To publish under your own name, change
`"publisher"` in `vscode/package.json` before rebuilding.

To try the extension without installing it, open the `vscode` folder in VS Code and press
`F5` to launch an Extension Development Host.

To uninstall: Extensions view > Sage Language > **Uninstall**.

---

## 8. Other editors

Sage files end in `.sage`. Any editor can run them: save the file and run `sage file.sage` in
a terminal. What the editors below add is colours and (for some) a run key. Only the Vim
syntax file was checked automatically (with a real Vim); the Emacs and Sublime files are
written to those editors' documented formats but have **not** been tried in the editors
themselves, and the notes for JetBrains, Notepad++ and Helix are untested suggestions.
Tell me what you see if something does not work.

All support files are in the `editors/` folder of the project.

### Settings that matter in every editor

- Indent with **4 spaces**, never tab characters: Sage blocks are defined by indentation.
- Comments start with `#`.
- Run with `sage yourfile.sage`; errors print as `Error in FILE on line N: message`.

### Vim and Neovim

Copy the three folders into your Vim runtime folder:

```
# Vim (Linux/macOS)
mkdir -p ~/.vim && cp -r editors/vim/* ~/.vim/
# Neovim
mkdir -p ~/.config/nvim && cp -r editors/vim/* ~/.config/nvim/
```

Open a `.sage` file: you get colours, 4-space indentation and **F5** to save and run. Make sure
`syntax on` and `filetype plugin indent on` are in your `vimrc` / `init.vim`.

### Emacs

```
mkdir -p ~/.emacs.d/lisp && cp editors/emacs/sage-mode.el ~/.emacs.d/lisp/
```

Add to your init file:

```
(add-to-list 'load-path "~/.emacs.d/lisp")
(require 'sage-mode)
```

`.sage` files open in Sage mode with colours; **C-c C-c** saves and runs the file.

### Sublime Text

1. Menu **Preferences > Browse Packages...**
2. Make a folder called `Sage` there and copy `editors/sublime/Sage.sublime-syntax` and
   `editors/sublime/Sage.sublime-build` into it.
3. Open a `.sage` file, pick **View > Syntax > Sage**, and run with **Ctrl+B** (`Cmd+B` on macOS).

### JetBrains IDEs (IntelliJ IDEA, PyCharm, WebStorm, ...)

These IDEs can read TextMate grammars. Open **Settings > Editor > TextMate Bundles**, click
**+**, and choose the project's `vscode` folder. `.sage` files should then be coloured. To
run files, add an **External Tool** (Settings > Tools > External Tools) with program `sage`
and arguments `$FilePath$`. (Untested.)

### Notepad++ (Windows)

Windows is easiest through WSL (see part 1). For colours, open **Settings > Style
Configurator**, choose the **Python** language, and add `sage` to its "User ext." box: Sage
looks close enough to Python that most of it will be coloured. Run with **Run > Run...** and
`cmd /k sage "$(FULL_CURRENT_PATH)"`. (Untested.)

### nano

Create `~/.nanorc` entries (or a file included from it):

```
syntax "sage" "\.sage$"
color brightblue "\<(let|if|else|elif|while|for|in|repeat|times|define|return|break|continue|pass|class|extends|try|catch|finally|raise|assert|use|import|from|as|with|global|nonlocal|lambda|and|or|not|is)\>"
color brightmagenta "\<(true|false|nothing|self|super|say)\>"
color green ""(\\.|[^"])*"" "'(\\.|[^'])*'"
color brightgreen "\<[0-9]+(\.[0-9]+)?\>"
color brightblack "#.*$"
```

### Helix, Zed, Kate, Gedit and others

None of these have a ready-made Sage package yet. Two options: treat `.sage` as Python for
rough colouring (in Helix, add `sage` to the Python `file-types` in `languages.toml`; in Zed, add
`"file_types": {"Python": ["sage"]}` to settings; in Kate and Gedit, choose Python from the
highlighting menu), or run Sage from the editor's built-in terminal. Editors that can import
TextMate grammars can use `vscode/syntaxes/sage.tmLanguage.json` directly.

---

## Troubleshooting

| Problem | Fix |
|---|---|
| Terminal says `sage: command not found` or `'sage' is not recognized` | The interpreter is not on PATH. Run `sudo make install`, or set `sage.executablePath` to the full path (step 4). Open a new terminal afterwards. |
| The file shows as "Plain Text", no colours | Click the language name at the bottom right and choose **Sage**. The file must end in `.sage`. |
| Play button missing | It only shows when a `.sage` file is the active tab and the extension is installed and enabled. |
| "Save the file as something.sage" | Untitled files cannot be run. Save the file first. |
| PowerShell shows a parse error about the path | Set `sage.executablePath` to a path without unusual characters, or switch the terminal to Git Bash / WSL. |
| `Error on line 7: ...` | That is a Sage error. The line number is in the file you ran. Common ones: a missing `:` after `if`/`define`, mixed tabs and spaces, or using a variable before it exists. |
| After installing, nothing changed | Run **Developer: Reload Window** from the Command Palette. |

If the extension still misbehaves, open **Help > Toggle Developer Tools** and check the
Console for lines mentioning `sage-lang`, and send them to me.
