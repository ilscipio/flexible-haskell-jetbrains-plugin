<!-- Plugin description -->
Full Haskell IDE support: HLS-powered completion and diagnostics, a GHCi REPL and debugger, 100 live templates, code inspections and refactorings, first-class Cabal support, and an optional AI assistant with a 32-tool MCP server.

> We are a young IDE extension and want to make it the best environment for Haskell developers.
>
> So expect it to
>
> a) still have the occasional bug
>
> b) improve quickly — we ship updates constantly.
>
> Please help us: leave a review, open a GitHub issue, or email us and let us know what you think, what works, and how to improve it further.

## Core Features

### Language Support
* **Complete Haskell grammar** for GHC Haskell syntax, including CPP preprocessor directives and `DataKinds` type-level literals
* **Bluespec support** — Bluespec Classic (`.bs`) shares the Haskell parser with hardware-specific keywords (`rule`, `interface`, `method`, `action`), and Bluespec SystemVerilog (`.bsv`) has a dedicated lexer for C-style comments and Verilog-sized literals
* **Cabal support** — syntax highlighting, completion, and documentation for `.cabal` and `cabal.project` files, with one-click `cabal-fmt` formatting
* **HLS (Haskell Language Server) integration** for completion, diagnostics, hover docs, inlay type hints, and code lenses (enable via Settings > Languages & Frameworks > Haskell)
* **Syntax highlighting** with a dedicated color settings page and tuned defaults for the Default, Darcula, and VS Code Dark+ themes
* **Code folding** for import blocks, functions, data types, classes, instances, and comments
* **Language injection** into QuasiQuotes (`[sql|…|]`, `[hamlet|…|]`, …) and string literals
* **Brace matching** with auto-close for `{`, `(`, `[`, **comment support** for line (`--`) and block (`{- … -}`) comments, and **Haddock** highlighting (`-- |`)
* **Project SDK** — configure a GHC / Stack / Cabal SDK from project settings, or download one via GHCup from Tools > Haskell

### Code Inspections & Quick Fixes
* **16 inspections** — unused/duplicate imports and exports, missing type signatures, possibly incomplete pattern matches, variable shadowing, unknown/unused `LANGUAGE` pragmas, and HLint-style smells (use camelCase, redundant bind, many imports)
* **6 intention actions** — add a missing import, add a type annotation, add a `LANGUAGE` pragma, add a `deriving` clause, toggle an explicit import list, and apply HLS refactorings
* **Import optimizer** (Ctrl+Alt+O), **postfix templates**, and **surround-with** support

### REPL, Run & Debug
* **Integrated GHCi REPL** with a JediTerm terminal widget in a tool window at the bottom of the IDE
* **Send to GHCi** (Alt+Shift+Enter) — send the selection or current line to the REPL
* **Load File in GHCi** (Ctrl+Shift+F10) — `:load` the current file
* **REPL controls** — Start, Stop, Restart, Interrupt, and Reload (Ctrl+Shift+F5) from Tools > Haskell
* **Clickable GHC stack traces** in the REPL console
* **Run configurations** with Stack / Cabal / runghc modes, an auto-producer for any `.hs` file, and a run gutter marker on `main`
* **GHCi debugger** with line breakpoints over Stack, Cabal, and runghc

### AI Assistant & MCP Server
* **AI chat tool window** — Claude Code, Codex, Cursor, Windsurf, Antigravity, Aider, or your own Anthropic / OpenAI / Gemini / Ollama key
* **MCP server** — 32 IDE tools spanning GHCi, workspace, code intelligence, tooling, and run/debug, installed into your agent CLI in one click
* **Fully optional** — one switch under Settings > Languages & Frameworks > Haskell > AI turns all of it off

### Navigation & Intelligence
* **Go to Symbol** (Ctrl+Alt+Shift+N) — navigate to any top-level declaration
* **Go to Class** (Ctrl+N) — navigate to data types, type aliases, type classes, and modules
* **Go to Declaration** (Ctrl+Click) and **Go to Implementation** (Ctrl+Alt+B) via HLS
* **Structure View** (Alt+7), **Call Hierarchy** (Ctrl+Alt+H), and **Find Usages** (Alt+F7)
* **Rename refactoring** (Shift+F6) for top-level identifiers
* **Library source navigation** — jump into GHC stdlib, Cabal/Stack dependencies, and downloaded Hackage sources
* **Gutter icons** for `module`, `data`, `newtype`, `class`, `instance`, and `type`

### Smart Completion & Documentation
* **LSP-powered completion** from Haskell Language Server, plus keyword, pragma, import, and qualified-name completion
* **LSP diagnostics** shown inline as editor annotations
* **Haddock documentation** (Ctrl+Q) — renders preceding `-- |` comments and type signatures, with basic markup (`@code@`, `/italic/`, `__bold__`)

### Formatting & Code Style
* **External formatters** — ormolu, fourmolu, or stylish-haskell via Format with External Formatter (Ctrl+Alt+Shift+F), plumbed into Reformat Code (Ctrl+Alt+L) with optional format-on-save
* **Built-in formatter** with spacing rules for operators (`->`, `=>`, `<-`, `::`, `=`, `|`), brackets, and commas
* **Code style settings** under Settings > Editor > Code Style > Haskell

### Live & File Templates
100 built-in live templates across 7 categories — General, IO, Typeclasses, Lens, Monad Transformers, Testing, and Concurrency. Type the abbreviation and press Tab:

* **Declarations:** `fn` (function + signature), `fnw` (with `where`), `dt`, `nt`, `cls`, `inst`
* **Expressions:** `case`, `do`, `let`, `where`, `if`, `lam`
* **Modules & Imports:** `mod`, `imp`, `imq`, `der`, `main`
* **Testing:** `test` (HUnit), `hspec`, `prop` (QuickCheck)
* …and dozens more for lenses (`vw`, `ov`, `lns`), monad transformers (`rdr`, `stt`, `wrt`), and STM/MVar concurrency (`stm`, `ntv`, `nmv`, `fork`)

File templates under New File — **Haskell Module**, **Haskell Script**, and **Haskell Test** — plus a **Haskell** entry in the New Project wizard that scaffolds a minimal `src/Main.hs`.

## Getting Started

### Quick Start
1. Install the plugin from JetBrains Marketplace
2. Configure your GHC/Stack/Cabal SDK under Settings > Languages & Frameworks > Haskell (or download one via Tools > Haskell > Download GHC SDK)
3. Create a new `.hs` file or open an existing Haskell project
4. Start coding with syntax highlighting, completion, and live templates
5. Press Alt+Shift+Enter to send code to the integrated GHCi REPL

### Productivity Tips
* **Type `fn` + Tab** for an instant function definition with type signature
* **Alt+Shift+Enter** to send the current selection to GHCi
* **Ctrl+Shift+F10** to `:load` the current file into GHCi
* **Ctrl+Q** on a function with a Haddock comment to see quick documentation
* **Alt+7** to open the Structure View for the current file
* **Ctrl+Alt+Shift+N** to jump to any top-level declaration
* **Ctrl+Alt+Shift+F** to format with ormolu / fourmolu / stylish-haskell

### Customization
* **Color scheme**: File > Settings > Editor > Color Scheme > Haskell
* **Live templates**: File > Settings > Editor > Live Templates > Haskell
* **Code style**: File > Settings > Editor > Code Style > Haskell
* **SDK path**: File > Settings > Languages & Frameworks > Haskell

### Troubleshooting
* **File not recognized?** Check Settings > Editor > File Types > Flexible Haskell — ensure `*.hs` is listed
* **No completions?** Make sure HLS is installed and the SDK path is set correctly in Haskell settings
* **GHCi not starting?** Verify `ghci` (or `stack exec ghci`) is on your system PATH

## Actively Developed

Flexible Haskell is a young project and we ship updates constantly. See the [full changelog](https://github.com/ilscipio/flexible-haskell-jetbrains-plugin/blob/main/CHANGELOG.md) for every release.

## We'd Love Your Feedback

Feature requests, bug reports, and general feedback are very welcome! You can reach us:

- **GitHub Issues:** [flexible-haskell-jetbrains-plugin](https://github.com/ilscipio/flexible-haskell-jetbrains-plugin/issues)
- **Email:** info@ilscipio.com
- **Website:** [ilscipio.com](https://www.ilscipio.com/)

## Made by Developers with a Passion for Functional Programming

The integration is the work of [Ilscipio](https://www.ilscipio.com/):

<p style="text-align:center">
<img src="https://www.ilscipio.com//wp-content/uploads/2018/11/ilscipio_soldier2-2.svg" width="200" alt="The Ilscipio Logo - A roman soldier"/>
</p>

We develop software solutions and understand the unique needs of Haskell developers. We created this plugin to make Haskell development more intuitive and productive in JetBrains IDEs.

We're sharing this tool with the Haskell community to support the ecosystem's growth and to make functional programming more accessible.

* Special discounts are available for individual developers & of course for the whole Open Source community.

## Bugs & Feature Requests

Found a bug or have a feature request? Please [open a GitHub issue](https://github.com/ilscipio/flexible-haskell-jetbrains-plugin/issues) — that's where we track and respond to reports. You can also reach us at info@ilscipio.com or via [Ilscipio](https://www.ilscipio.com/).
<!-- Plugin description end -->
