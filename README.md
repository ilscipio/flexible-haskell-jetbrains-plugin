<!-- Plugin description -->
Haskell IDE with GHCi REPL, HLS integration, step debugger, smart completion, code inspections, live templates, and more. Full support for GHC Haskell syntax, pure functional programming, type classes, monads, and Haskell's unique language features.

**We are actively building this plugin and want to make it the best environment for Haskell developers. Please leave a review, open a GitHub issue, or drop us an email — your feedback shapes the roadmap.**

## Core Features

### Language Support
* **Complete Haskell grammar** with support for GHC Haskell syntax including language pragmas, type families, and GADTs
* **Advanced syntax highlighting** with three customizable color schemes (Default, Darcula, VS Code Dark+)
* **Code folding** for import blocks, function bodies, data declarations, type classes, instances, and comment runs
* **Brace matching** with auto-close for `{`, `(`, `[` and navigation between pairs
* **Comment support** for line comments (`--`) and block comments (`{- ... -}`)
* **Haddock highlighting** — `-- |` documentation comments rendered distinctly
* **Literate Haskell** — `.lhs` files recognized and syntax-highlighted
* **Breadcrumbs navigation** showing the current module, function, and declaration hierarchy

### GHCi REPL & Build Tools
* **Integrated GHCi REPL** with full terminal emulation via JediTerm
* **Send to GHCi** (Alt+Shift+Enter) — send the current selection or current line to the running REPL
* **Load File in GHCi** (Ctrl+Shift+F10) — `:load` the current file into the interactive session
* **Stack / Cabal / runghc** run modes selectable per run configuration
* **Clickable GHC stack traces** in the REPL console — click any file path to jump to the error location
* **REPL controls** — Start, Stop, Restart, and Interrupt from the Tools menu or toolbar

### Run / Debug Configurations
* **Haskell run configuration** with file path, program arguments, working directory, and run mode
* **Run configuration producer** — auto-creates a run configuration from context for any `.hs` file
* **Run line marker** — green run gutter icon on `main` for one-click execution
* **GHCi debugger** with full breakpoint support:
  * Click any line in the gutter to set a red-dot breakpoint
  * **Step Into (F7)** — enter sub-expressions and called functions
  * **Step Over (F8)** — advance to the next source line
  * **Resume (F9)** — continue to the next breakpoint or program end
  * **Variables panel** — inspect local bindings and their current values at every pause
  * Automatic breakpoint relocation when clicking a non-expression line (type signatures, blank lines)

### Haskell Language Server (HLS) Integration
* **LSP-powered completions** — full identifier and type suggestions from HLS
* **Live diagnostics** — HLS errors and warnings shown as inline editor annotations
* **Hover documentation** — type signatures and Haddock rendered on Ctrl+Q or mouse hover
* **Go to Declaration** (Ctrl+Click) — jump to the definition of any identifier via LSP
* **Status bar widget** — shows HLS state (Starting / Ready / Error / Degraded)
* **GHC version display** — current GHC version shown in the status bar
* **Restart HLS** — Tools > Haskell > Restart HLS clears the cache and relaunches

### Navigation & Intelligence
* **Go to Symbol** (Ctrl+Alt+Shift+N) — jump to any top-level declaration: functions, data types, type classes
* **Go to Class** (Ctrl+N) — navigate to data types, newtype, type aliases, type classes, and modules
* **Structure View** (Alt+7) — tree of module, functions, data types, classes, and instances with icons
* **Find Usages** (Alt+F7) — find all references to a symbol across your project
* **Gutter icons** — distinct icons for `module`, `data`, `newtype`, `class`, `instance`, and `type` declarations
* **Rename refactoring** (Shift+F6) — rename any top-level identifier

### Smart Completion
* **Keyword completion** — all Haskell keywords with context awareness
* **Pragma completion** — `{-# LANGUAGE ... #-}`, `{-# OPTIONS_GHC ... #-}` with known extension/option names
* **Import completion** — module names with qualified and hiding forms
* **Qualified name completion** — `Map.` → shows exported names for known qualified imports
* **Snippet completion** — common multi-token patterns inserted as full snippets
* **HLS completion** — full identifier and type completions from the running language server (runs last, highest priority)

### Code Inspections & Quick Fixes
**Six automatic inspections with Alt+Enter quick fixes:**
* **Missing type signature** — warns when a top-level function lacks a `::` annotation; quick fix inserts a placeholder signature
* **Unused import** — warns on imports where no names from the module appear in the file; quick fix removes the import
* **Redundant pragma** — warns on `{-# LANGUAGE ... #-}` pragmas that are duplicated or known to be implied; quick fix removes them
* **Variable shadowing** — warns when a local binding shadows a name from an outer scope
* **Incomplete pattern** — heuristic warning on `case` expressions with only one branch and no wildcard `_`
* **Unused binding** — warns on `let` / `where` bindings that are never referenced

### Live Templates (~99 templates)
Type the abbreviation and press Tab. Templates are organized in seven groups under **Settings > Editor > Live Templates > Haskell**:

**General (20 templates):**
- `fn` — Function definition with type signature
- `fnw` — Function with `where` clause
- `dt` — Data type declaration
- `nt` — Newtype with accessor
- `cls` — Type class declaration
- `inst` — Instance declaration
- `case` — Case expression with wildcard
- `do`, `let`, `if`, `lam` — Common expressions
- `mod`, `imp`, `imq`, `der`, `main` — Module, import, and entry-point forms
- `test`, `hspec`, `prop` — Testing templates

**IO (10 templates):** `io`, `hGetLine`, `hPutStr`, `readFile`, `writeFile`, `withFile`, `bracket`, `finally`, `catch`, `throwIO`

**Typeclass (10 templates):** `functor`, `applicative`, `monad`, `foldable`, `traversable`, `semigroup`, `monoid`, `eq`, `ord`, `show`

**Lens (15 templates):** `lens`, `view`, `set`, `over`, `makeLenses`, `makePrisms`, `iso`, `prism`, and more

**Monad (15 templates):** `stateT`, `readerT`, `writerT`, `exceptT`, `runState`, `runReader`, `ask`, `tell`, `lift`, and more

**Test (15 templates):** `testCase`, `testGroup`, `testSpec`, `describe`, `it`, `shouldBe`, `shouldReturn`, `arbitrary`, `forAll`, and more

**Concurrency (14 templates):** `forkIO`, `newMVar`, `takeMVar`, `putMVar`, `newTVar`, `readTVar`, `writeTVar`, `atomically`, `async`, `wait`, and more

### Postfix Templates
Type `.` after any expression to transform it:
- `.map` → `map (\x -> x) expr`
- `.filter` → `filter (\x -> x) expr`
- `.case` → `case expr of { _ -> }`
- `.let` → `let x = expr in x`
- `.show` → `show expr`
- `.print` → `print expr`
- `.pure` → `pure expr`
- `.return` → `return expr`
- `.not` → `not expr`
- `.par` → `(expr)`

### Surround With (7 templates)
Select any expression and press **Ctrl+Alt+T** (or use Code > Surround With):
- **Parentheses** — `(expr)`
- **Let-in** — `let x = expr in ...`
- **Do** — `do { expr; ... }`
- **Case-of** — `case expr of { _ -> ... }`
- **If-then-else** — `if expr then ... else ...`
- **Where** — wraps the expression in a `where` binding
- **Lambda** — `\x -> expr`

### Code Formatting
* **Basic formatter** with spacing rules for Haskell operators (`->`, `=>`, `<-`, `::`, `=`, `|`), brackets, and commas
* **External formatter** integration — configure an external tool (e.g. Ormolu, Fourmolu, Brittany) in settings
* **Format on save** — automatically run the formatter when saving a file (enable in settings)
* **Format action** — Ctrl+Alt+Shift+F to format the current file on demand
* **Import optimizer** (Ctrl+Alt+O) — sorts and deduplicates imports

### SDK Management
* **GHC / Stack / Cabal SDK type** with automatic detection from PATH and common install locations
* **Auto-configuration** on startup — detected tools are registered automatically; project SDK set if none is configured
* **GHCup integration** — Tools > Haskell > Download GHC SDK opens a dialog listing available GHC versions; installs via `ghcup install`
* **SDK notification banner** — if no SDK is configured, a banner offers to detect or download one
* **GHC stdlib sources** — GHC library sources are registered as a synthetic library for navigation

### New Project Wizard & File Templates
* **Haskell** appears in the New Project wizard with automatic `src/Main.hs` generation
* **File templates** available under File > New:
  * **Haskell Module** — `module Name where` skeleton
  * **Haskell Script** — `module Main` with `main :: IO ()` body
  * **Haskell Test** — HSpec or HUnit test module skeleton

---

## Getting Started

### Quick Start
1. Install the plugin from JetBrains Marketplace
2. Install GHC via GHCup or your system package manager
3. Configure the SDK under **Settings > Languages & Frameworks > Haskell**
4. Open a `.hs` file — syntax highlighting, folding, and structure view activate immediately
5. Enable HLS for completions and diagnostics (requires `ghcup install hls`)
6. Press **Alt+Shift+Enter** to send code to the integrated GHCi REPL

### Productivity Tips
* **Type `fn` + Tab** for an instant function definition with type signature
* **Type expression + `.map` + Tab** for postfix completion
* **Alt+Enter** on any highlighted element for context-aware quick fixes
* **Ctrl+Q** on a function name for Haddock documentation
* **Alt+7** to open Structure View for the current file
* **Ctrl+Alt+Shift+N** to jump to any top-level declaration
* **Ctrl+Click** on an identifier (with HLS running) to go to its definition

### Customization
* **Color schemes**: File > Settings > Editor > Color Scheme > Haskell (Default, Darcula, VS Code Dark+)
* **Live templates**: File > Settings > Editor > Live Templates > Haskell
* **Code style**: File > Settings > Editor > Code Style > Haskell
* **Inspections**: File > Settings > Editor > Inspections > Haskell
* **SDK & HLS settings**: File > Settings > Languages & Frameworks > Haskell

### Troubleshooting
* **File not recognized?** Check Settings > Editor > File Types > Flexible Haskell — ensure `*.hs` is listed
* **No completions?** Make sure HLS is installed (`ghcup install hls`) and the SDK path is set correctly
* **GHCi not starting?** Verify that `ghci` (or `stack exec ghci`) is available on your system PATH
* **Debugger not stopping?** Ensure the breakpoint is on an expression line, not a type signature or import

---

## What's New in 2026.1.0

- **GHCi debugger** — breakpoints, step into / over / resume, and variables panel powered by GHCi's built-in tracing
- **HLS integration** — completions, live diagnostics, hover docs, and Go to Declaration via LSP
- **Six code inspections** — missing type signature, unused imports, redundant pragma, shadowing, incomplete pattern, unused binding
- **~99 live templates** across seven groups (General, IO, Typeclass, Lens, Monad, Test, Concurrency)
- **10 postfix templates** and **7 surround-with templates**
- **External formatter** integration with format-on-save
- **VS Code Dark+ color scheme**
- **Breadcrumbs**, postfix templates, refactoring support, and import optimizer

See [CHANGELOG.md](CHANGELOG.md) for complete details.

---

## Made by Developers with a Passion for Functional Programming

The integration is the work of [Ilscipio](https://www.ilscipio.com/):

<p style="text-align:center">
<img src="https://www.ilscipio.com//wp-content/uploads/2018/11/ilscipio_soldier2-2.svg" width="200" alt="The Ilscipio Logo - A roman soldier"/>
</p>

We develop software solutions and understand the unique needs of Haskell developers. We created this plugin to make Haskell development more intuitive and productive in JetBrains IDEs. We are sharing this tool with the Haskell community to support the ecosystem's growth and to make pure functional programming more accessible.

* Special discounts are available for individual developers & of course for the whole Open Source community.

## Bugs & Feature Requests

If you have any questions, feature requests, or stumble upon the occasional bug, come leave us a message:

- **GitHub Issues:** [flexible-haskell-jetbrains-plugin](https://github.com/ilscipio/flexible-haskell-jetbrains-plugin)
- **Email:** info@ilscipio.com
- **Website:** [ilscipio.com](https://www.ilscipio.com/)
<!-- Plugin description end -->
