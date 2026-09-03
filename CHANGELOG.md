# Changelog

## [Unreleased]

## [2026.1.64] - 2026-09-03

### Fixed

- GHC `DataKinds` types now parse in type positions: literals (`V 2 Rational`, `type SigEpsNumCrossing = 5`), promoted constructors (`'Just`, `'Nothing`), promoted lists/tuples (`'[Int, Bool]`, `'(Int, Bool)`), and promoted operators (`a ': b`, `'(:)`) (Issue #49, @vasdommes)

### Thanks

- @vasdommes

## [2026.1.63] - 2026-09-02

### Fixed

- Pasting a multi-line type synonym or type signature no longer flattens its indented continuation lines to column 0, which previously turned valid layout into a parse error (Issue #48, @vasdommes)

### Thanks

- @vasdommes

## [2026.1.62] - 2026-09-02

### Fixed

- Postfix `qualified` (GHC `ImportQualifiedPost`) and operator type constructors with a member list now parse, fixing errors on `import Data.Map.Strict qualified as Map` and `import Data.Type.Equality ((:~~:) (..))` (Issue #46, @vasdommes)
- `forall` binders can now carry a kind signature (GHC `KindSignatures`), fixing the parse error on `forall r (p :: Nat) b. …` in type signatures and existential constructors (Issue #48, @vasdommes)
- Type synonyms can now carry a top-level kind signature on their right-hand side, fixing the parse error on `type T b = (forall p. …) :: Constraint` (Issue #48, @vasdommes)

### Thanks

- @vasdommes

## [2026.1.61] - 2026-08-28

### Added

- AI chat tool window "Haskell AI" with agent CLI backends (Claude Code, Codex, Gemini, Cursor, Windsurf, Antigravity, Aider) and external API providers (Anthropic, OpenAI, Gemini, Ollama)
- MCP server on port 8577 that gives an agent 32 IDE tools for GHCi, workspace, code intelligence, tooling and run/debug, shared across every open project
- Settings page Haskell > AI to pick the backend, manage API providers, start or stop the MCP server, and install it into a detected agent CLI
- Setting "Enable AI features" that removes the AI tool window and the MCP server completely

### Thanks

- Internal improvement

## [2026.1.60] - 2026-08-25

### Fixed

- Block expressions (`do`, `case`, `\`, `if`, `let … in`) can now be passed directly as a function's last argument without `$` or parens (GHC `BlockArguments` extension), fixing parse errors like `<layout stmts> … expected` on `liftIO do …` (Issue #47, @saeidscorp)

### Thanks

- @saeidscorp

## [2026.1.59] - 2026-08-25

### Fixed

- `type` keyword is now accepted in import and export lists (GHC `ExplicitNamespaces` extension), fixing the parse error for items like `type (:>)` (Issue #46, @saeidscorp)
- Import spec `(...)` on the next line after the module name now parses correctly even when the opening `(` is at column 0 (Issue #46, @saeidscorp)

### Thanks

- @saeidscorp

## [2026.1.58] - 2026-08-24

### Added

- Setting "Open library sources as real files" (default on) that opens downloaded library sources from their cached file on disk instead of a read-only in-memory copy; disable it if opening library files is slow on Windows

### Fixed

- Go to Declaration and Cmd/Ctrl-click now work on symbols inside library sources, and hovering highlights them, instead of showing "Cannot find declaration to go to" (Issue #43, @tbodor)
- IdeaVim now activates in library sources and the files appear under External Libraries, because they open as real files rather than in-memory copies (Issue #43, @tbodor)
- A `.git` boundary marker is backfilled into every cached package so opening a library file can no longer trigger a VCS scan that freezes the IDE on Windows (Issue #43, @tbodor)

### Thanks

- @tbodor

## [2026.1.57] - 2026-08-21

### Added

- Find Usages and Go to Declaration for qualified-import aliases: invoking either on `TL` in `import qualified Data.Text.Lazy as TL`, or on the `TL` qualifier of a use such as `TL.fromStrict`, now finds or navigates to the alias and every qualified use in the file

### Changed

- Completely rewrote the lexer and parser from the ground up for accurate, whole-file understanding of real-world Haskell — full layout/offside handling and broad GHC-extension coverage — giving navigation, inspections, folding, and documentation a reliable foundation in place of the previous limited implementation

### Fixed

- Layout: `else`/`then`/`of` on a continuation line, and a `where` following a `\case`/`case … of` block, no longer produce spurious parse errors, so multi-line `if`/`case` expressions inside a `do` block parse cleanly
- A comment between two declarations is no longer captured by the preceding `do`/`where`/`let` block, so code folding stops at the function body and quick documentation shows the following declaration's Haddock comment
- Aliased qualified imports (`import qualified M as X`) are no longer flagged as unused, and Find Usages no longer throws a read-access error on background threads

### Thanks

- Internal improvement

## [2026.1.56] - 2026-08-18

### Fixed

- Go to Declaration on implicitly-imported Prelude symbols now lands on the real definition: operators such as `>>=` and functions such as `flip`/`putStrLn` follow the module re-export chain with an operator-aware source search instead of failing or opening the module at its top, while same-file local bindings and explicit imports keep priority (Issue #43, @tbodor)
- Navigating into external library sources now reuses the existing editor tab instead of opening a duplicate every time (Issue #43, @tbodor)
- Quick documentation now shows the real HLS/Haddock for imported symbols, falling back to the bundled docs only when HLS is unavailable (Issue #43, @tbodor)

### Thanks

- @tbodor for the detailed report and reproduction

## [2026.1.55] - 2026-08-17

### Fixed

- Go-to-definition / Ctrl-click on an import's module name (e.g. `Control.Monad`) now resolves; the name is wrapped in a `QUALIFIED_NAME` node, which the resolver did not look through
- Import, module-header and qualified export names are highlighted again as module/export references
- Duplicate-export detection no longer misses type exports, and the Add-import intention no longer offers a module that is already imported

### Thanks

- Internal improvement

## [2026.1.54] - 2026-08-17

### Fixed

- Layout parse-error rule: a closing bracket on its own line, a `where` after a `do` block, and an infix operator continuing the previous line no longer produce spurious layout errors

### Thanks

- Internal improvement

## [2026.1.53] - 2026-08-17

### Fixed

- Parse eight common GHC extensions: ExistentialQuantification, KindSignatures, TypeApplications, ViewPatterns, TupleSections, DerivingVia, GADTs, and Type/Data families
- Added a `corpusScan` Gradle task that batch-parses a Haskell checkout and reports every parser error node at once

### Thanks

- Internal improvement

## [2026.1.52] - 2026-08-17

### Fixed

- MultiWayIf (`if | guard -> expr | guard -> expr`) now parses; the guard arms were mistaken for do-block structure, causing "`<layout stmts>` or `{` expected"

### Thanks

- Internal improvement

## [2026.1.51] - 2026-08-17

### Fixed

- Qualified constructor patterns with arguments (`E.SomeException e`, `Foo.Bar x y`) now parse; the pattern variable after the constructor was rejected with "`)` or `,` expected, got 'e'"

### Thanks

- Internal improvement

## [2026.1.50] - 2026-08-17

### Fixed

- Multi-param type class entries in `deriving` lists (`MonadBase b`, `MonadBaseControl b`) now parse; type arguments after the class name were rejected with "`)` or `,` expected, got 'b'"

### Thanks

- Internal improvement

## [2026.1.49] - 2026-08-17

### Fixed

- DerivingStrategies extension (`deriving stock`, `deriving newtype`, `deriving anyclass`) now parses; the optional strategy keyword before the class list (and before `instance` in standalone deriving) was rejected with "CONID expected, got 'newtype'"

### Thanks

- Internal improvement

## [2026.1.48] - 2026-08-14

### Fixed

- Pattern type signatures (`(x :: Int)`, ScopedTypeVariables) now parse in patterns — function arguments, lambdas, `case` alternatives, and `let`/`where` bindings; a trailing `::` in a pattern was previously rejected

### Thanks

- Internal improvement

## [2026.1.47] - 2026-08-14

### Fixed

- LambdaCase (`\case` with its alternatives) now parses; the lexer opens a layout block after `\case` as it does after `of`, and `lambda_expr` accepts the case-alternative form

### Thanks

- Internal improvement

## [2026.1.46] - 2026-08-14

### Fixed

- RecordWildcards (`Con { .. }` and `Con { field = x, .. }`) now parse in both patterns and record construction; the `..` was rejected, so a record-wildcard function argument fell into an error
- Qualified names in module export lists (`module M ( …, Q.foo )`) now parse; only unqualified variables and qualified type names were accepted, so a re-exported qualified value errored

### Thanks

- Internal improvement

## [2026.1.45] - 2026-08-14

### Fixed

- Editor no longer throws "Lexer is not progressing after calling advance()" when opening files with empty layout blocks (`class C where` with no members, `case x of {}`) or nested dedents; syntax highlighting and the find-usages word scanner now use a lexer variant that omits the parser-only zero-width virtual layout tokens

### Thanks

- Internal improvement

## [2026.1.44] - 2026-08-14

### Added

- Unicode letters are now accepted in identifiers (`café`, `σum`, `Δ`) per the Haskell 2010 lexical syntax; the lexer was ASCII-only

### Fixed

- Parenthesised function LHS (`(f x) y = …`), fixity declarations inside a class body, qualified field names in record patterns/construction (`R{M.f = x}`), and empty `case` expressions (`case x of {}`) now parse instead of producing error-recovery nodes

### Thanks

- Internal improvement

## [2026.1.43] - 2026-08-14

### Added

- Go-to-definition now resolves local variables to their binding site — function and lambda parameters, `let`/`where` bindings, `case`-alternative patterns, do-block `pat <- e` / `let` statements, and list-comprehension qualifiers (`[e | x <- xs, let y = …]`) — walking outward so an inner binding shadows an outer one or a module-level name

### Thanks

- Internal improvement

## [2026.1.42] - 2026-08-14

### Fixed

- Structure view now lists class and instance methods, and go-to-definition resolves same-file class/instance methods; both scanned only a declaration's direct children for equations, but GrammarKit nests methods — including signature-only ones — inside `CLASS_BODY`/`INST_BODY`
- do-blocks with three or more statements now fold; the builder counted statements as direct children of the do-expression, but they are wrapped in a `LAYOUT_STMTS` node so the count was always zero
- Type-operator declarations (`data a :+: b`, `type a ~> b`) are now navigable and renamable and show in the structure view; name extraction recognised only alphabetic constructors, so the operator name was invisible to go-to-definition, find-usages, and rename

### Thanks

- Internal improvement

## [2026.1.41] - 2026-08-13

### Fixed

- As-patterns (`xs@(x:_)`) and record patterns (`Person { name = n }`) now parse; the pattern grammar matched the bare variable/constructor first and errored on the following `@`/`{`, breaking navigation and inspections for those equations
- Lambda, let, if, case, and do expressions can now be operator operands (`f $ \x -> e`, `xs >>= \x -> …`, `x + if c then a else b`); previously only plain application was allowed as an operand, so these collapsed into error-recovery nodes
- Type operators (`data a :+: b`, `type a ~> b`, `instance C (:+:)`), infix constructors (`a \`Branch\` Tree a`), functional dependencies (`class C a b | a -> b`), empty data declarations (`data Void`), strict type annotations (`!Int`), empty class/instance bodies, `let` in list-comprehension qualifiers (`[y | x <- xs, let z = f x]`), and CPP directives interspersed with code now parse instead of producing error-recovery nodes

### Thanks

- Internal improvement

## [2026.1.40] - 2026-08-13

### Fixed

- Go-to-definition, find-usages, and quick documentation now work for class and instance methods, including signature-only methods; instance bodies were wrapped in an extra `INST_WHERE` node and tree consumers only matched `FUNCTION_DEF`, so instance methods and bare method signatures were invisible
- Infix operator definitions such as `a .+. b = a + b` now parse as a single equation instead of collapsing into an error-recovery `DUMMY_BLOCK`; this also removes a false "missing type signature" warning for operator definitions under `PackageImports`
- Rename now works on function definitions (it was a no-op after the GrammarKit migration), the caller hierarchy no longer lists a standalone function as its own caller, and `import Foo ()` now produces an `IMPORT_LIST` node

### Thanks

- Internal improvement

## [2026.1.39] - 2026-08-13

### Fixed

- `let x = e in expr` on a single line now parses correctly; the layout lexer was not emitting `VIRTUAL_RBRACE` before `in` when it appeared on the same line as the last binding, causing the body expression to fall into a `DUMMY_BLOCK` with no enclosing `FUNCTION_DEF`
- Ctrl+W (extend selection) now expands through meaningful intermediate ranges: paren content, function application, case alternative body, case alternative, case expression, if-then-else, let-in, do block, lambda, and function RHS; the previous handler skipped all intermediate steps and jumped straight to the whole file (Issue #16)
- Unused import inspection now compiles correctly after the GrammarKit migration; an `IElementType` import was missing from `HaskellUnusedImportInspection`

### Thanks

- Internal improvement

## [2026.1.38] - 2026-08-12

### Fixed

- GHCi `:reload` action added to the REPL toolbar (Ctrl+Shift+F5); the service method existed but had no UI entry point
- `getAllFunctionNames/TypeNames/ModuleNames` now return real results by PSI-scanning all `.hs`/`.lhs` files in project scope with a `PsiModificationTracker`-backed cache; they previously returned empty lists, silently breaking completion contributors and navigation that call them
- `HaskellUnusedBindingInspection` quick fix now deletes the binding declaration; it was a no-op body
- `stack.yaml` and `package.yaml` now display the Haskell module icon in the project tree; their file type is unchanged so the YAML plugin's syntax highlighting is preserved

### Thanks

- Internal improvement

## [2026.1.37] - 2026-08-12

### Fixed

- `textDocument/documentHighlight` now backed by HLS; Ctrl+Shift+F7 highlights all semantic usages with read/write distinction instead of text-only scanning
- `window/logMessage` notifications from HLS are now forwarded to the IDE log (error/warning → WARN, info → INFO, log → DEBUG)
- `initializeParams` now declares all missing client capabilities: `synchronization.save`, `documentHighlight`, `typeDefinition`, `foldingRange`, `selectionRange`, `codeAction.resolveSupport`, `completion.completionItem.resolveSupport`, `workspace.executeCommand`, `window.workDoneProgress`

### Thanks

- Internal improvement

## [2026.1.36] - 2026-08-12

### Fixed

- Inlay type hints now render; `collect()` returned false before the file-level collection ran, so no hints were ever shown
- Cross-file rename now applies HLS workspace edits to all files; previously only the symbol under the cursor changed
- `textDocument/didSave` is now sent on every file save so HLS re-analyses after saves instead of serving stale diagnostics
- Formatting provider setting (ormolu/fourmolu/stylish-haskell/hindent) is now forwarded to HLS on startup; it was hardcoded to ormolu
- Incomplete pattern match inspection re-enabled; it was disabled for a stale reason (parser does have `CASE_EXPR`/`ALT` nodes)
- Backtick auto-close added; typing `` ` `` now inserts a matching closing backtick for infix operator expressions

### Thanks

- Internal improvement

## [2026.1.35] - 2026-08-12

### Fixed

- Nested `where`, `let`, `do`, and `of` blocks now parse correctly using the Haskell layout rule; bindings inside a nested `where` no longer leak as spurious top-level declarations in complex modules
- Compound operators such as `==`, `<>`, and `>>` are now a single token; the previous stub lexer split them into multiple `=` or `>` tokens, breaking operator highlighting and infix usage detection

### Thanks

- Internal improvement

## [2026.1.34] - 2026-08-10

### Fixed

- Go-to-definition into library sources now lands on the correct line and populates the structure view; the external viewer builds real Haskell PSI instead of a token-only tree (Issue #43)
- External navigation now resolves Cabal project dependencies, not just GHC boot libraries; the module map is built from the global DB plus the Cabal store package DB (Issue #43)
- Type signatures now parse as their own declaration instead of merging with the following equation, fixing a spurious missing-signature warning and inconsistent name highlighting for multi-name signatures like `inc, dec :: Int -> Int` (Issue #44)

### Thanks

- @tbodor for reporting #43 and #44

## [2026.1.33] - 2026-08-07

### Fixed

- Unknown LANGUAGE extension inspection now validates against the project's actual GHC extension list (or the bundled fallback) instead of a stale hardcoded set; extensions like OrPatterns no longer trigger false positives (Issue #42)

### Thanks

- Reporter of Issue #42

## [2026.1.32] - 2026-07-13

### Fixed

- Parser no longer hangs the IDE on a module export list containing an unrecognized token; export-item parsing now always consumes input and checks for cancellation (Issue #41, @develop7)

### Thanks

- @develop7 for the thread dump that pinpointed the parser hang

## [2026.1.31] - 2026-07-13

### Fixed

- HLS no longer fails to start when installed inside WSL2; now launched via wsl.exe instead of an unexecutable UNC path (Issue #40, @vasdommes)
- Picker dropdowns in Toolchain settings now correctly expand width on Windows to show full paths when opened (Issue #40, @vasdommes)

### Thanks

- @vasdommes for reporting both regressions in 2026.1.30

## [2026.1.30] - 2026-07-08

### Added

- Toolchain settings path fields replaced with inline browse buttons on each picker row; click `...` to set a custom executable path

### Fixed

- Picker dropdowns in Toolchain settings now expand to show full paths when opened

## [2026.1.29] - 2026-07-07

### Added

- Toolchain settings now show a dropdown of all detected GHC and HLS installations so you can pick between multiple versions without editing paths manually (Issue #40, @vasdommes)

### Fixed

- Auto-detect toolchain no longer freezes the UI with an EDT synchronous-execution error on 2026.1+ (Issue #40, @vasdommes)
- Auto-detect now finds GHC, HLS, Stack, and Cabal installed via GHCup inside WSL2 on Windows (Issue #40, @vasdommes)

### Thanks

- @vasdommes for reporting the WSL toolchain detection issue and the EDT crash

## [2026.1.28] - 2026-06-24

### Added

- Rename parameter/local variable via Refactor > Rename (Issue #39, @cwcowell)

### Thanks

- @cwcowell for reporting and testing the rename feature

## [2026.1.27] - 2026-06-16

### Added

- New Project wizard now appears in PyCharm, CLion, WebStorm, and all other non-IDEA JetBrains IDEs with GHC SDK selection

## [2026.1.26] - 2026-06-15

### Fixed

- Autocomplete suggestions no longer appear inside line comments, block comments, and Haddock comments (Issue #37, @cwcowell)

### Thanks

- @cwcowell for reporting the comment completion issue

## [2026.1.25] - 2026-06-13

### Fixed

- Lexer no longer throws InvalidStateException when typing inside unterminated string literals (Issue #36, @cwcowell)
- Diagnostic tooltips now preserve newlines and indentation from GHC error messages (Issue #34, @cwcowell)
- Accepting a suggested type signature no longer triggers a spurious HLS parse error notification (Issue #35, @cwcowell)

### Thanks

- @cwcowell for reporting the various issues

## [2026.1.24] - 2026-06-02

### Fixed

- PackageImports extension no longer causes false "missing type signature" warnings when combined with infix operator definitions (Issue #33, @bristermitten)
- Nix/direnv environments are now detected automatically; GHC, HLS, Stack, and Cabal are resolved from the project shell instead of only hardcoded paths (Issue #32, @bristermitten)

### Thanks

- @bristermitten for reporting both the PackageImports parsing bug and the nix/direnv support request

## [2026.1.23] - 2026-05-31

### Fixed

- LSP completion items are now filtered by the typed prefix instead of showing all results regardless of input (Issue #30, @cwcowell)
- Rename refactoring (Shift+F6) is no longer grayed out on functions, data types, newtypes, type aliases, and classes (Issue #31, @cwcowell)
- Stack projects now use `stack exec -- ghc-pkg dump` to discover all snapshot packages instead of only the global DB, fixing the "16 deps downloaded" issue (Issue #28, @develop7)
- Downloaded Hackage sources now appear under External Libraries on subsequent project opens, not just the first session (Issue #28, @develop7)
- Local packages and git dependency sources from stack.yaml are exposed as library roots for navigation (Issue #28, @develop7)

### Added

- Cabal project dependency discovery via build-depends parsing
- .hie file directory scanning for future navigation support

### Thanks

- @cwcowell for reporting the completion filtering bug and the grayed-out rename
- @develop7 for the detailed dependency/library feedback

## [2026.1.22] - 2026-05-27

### Added

- Scope completion: typing inside a function body now suggests parameter names, where-clause bindings, and let bindings as high-priority completion items; toggle via Settings > Haskell > General (Issue #29, @Cmdv)

### Thanks

- @Cmdv for the feature request and kind words about the plugin

## [2026.1.21] - 2026-05-24

### Fixed

- HaskellLexer no longer infinite-loops on unterminated string literals at end of line; the lexer now recovers at the newline, so indexing of files with malformed strings no longer hangs the IDE (Issue #28, @develop7)

### Thanks

- @develop7 for the thread dumps that pinpointed the lexer hang

## [2026.1.20] - 2026-05-18

### Fixed

- HLS path setting is now applied immediately; changing the path in Settings restarts the language server with the new binary instead of ignoring it until IDE restart (Issue #26, @jedediah)
- Autocomplete now provides project-local symbols (functions, types, classes) from the current file and other project files as fallback completions, and import completion discovers project module names alongside the built-in module list (Issue #27, @vasdommes)

### Thanks

- @jedediah for reporting the HLS path setting being ignored
- @vasdommes for reporting the empty autocomplete

## [2026.1.19] - 2026-05-15

### Added

- Go to definition now navigates into external library source code, downloading Hackage source tarballs on demand and indexing them as project libraries for instant subsequent lookups (Issue #25, @develop7)

### Thanks

- @develop7 for the feature request and prior art research

## [2026.1.18] - 2026-06-08

### Fixed

- "Add deriving clause" and other intentions now use filterable popups instead of modal dialogs, fixing keyboard input under JetBrains Gateway / remote dev over SSH
- GHC download dialog no longer spawns nested modal dialogs; uses in-dialog state and balloon notifications for status messages

### Thanks

- @vasdommes for the original Gateway report that drove these fixes

## [2026.1.17] - 2026-04-28

### Fixed

- Intention preview crashed with "Intention Description Dir URL is null" when showing context actions via Alt+Enter (Issue #24, @develop7)

### Thanks

- @develop7 for reporting the unhandled exception

## [2026.1.16] - 2026-04-20

### Added

- Improved Haskell native formatting (Issue #22, @vasdommes)

### Fixed

- Extend Selection (Ctrl+W) now steps through import item, import list, import declaration, and all imports for cursors inside import statements (Issue #21, @vasdommes)
- Extend Selection (Ctrl+W) now selects the enclosing instance or class declaration after selecting a function definition inside a where clause (Issue #21, @vasdommes)

### Thanks

- @vasdommes for reporting both issues

## [2026.1.15] - 2026-04-16

### Fixed

- "Add LANGUAGE pragma" intention now shows a filterable popup of GHC extensions instead of a modal input dialog, which did not accept keyboard input under JetBrains Gateway / remote dev over SSH (@vasdommes)
- LANGUAGE pragma completion and the popup now source extensions from `ghc --supported-extensions` on the configured SDK, so the list matches the user's actual GHC version; a bundled fallback covers the no-SDK case, and a "custom" entry always lets the user type any extension name

### Thanks

- @vasdommes for reporting the broken input dialog under remote dev over SSH

## [2026.1.14] - 2026-04-15

### Fixed

- "Remove unused pragma" quickfix now removes the full pragma including {-# symbols (Issue #20, @vasdommes)

### Thanks

- @vasdommes for reporting the pragma removal issue

## [2026.1.13] - 2026-04-10

- Bluespec Classic (.bs) support with Haskell-shared parser and Bluespec-specific keyword highlighting
- Bluespec SystemVerilog (.bsv) support with dedicated lexer handling // and /* */ comments, Verilog-sized literals like 4'b1010 and 16'hFF, strings, keywords, and type names 
- .bs files now participate in module resolution, Go to Symbol, and Go to Class navigation alongside .hs and .lhs

### Thanks

- @anonymous for asking for Bluespec support

## [2026.1.12] - 2026-04-08

### Fixed

- Number highlighting now handles underscores from NumericUnderscores extension, e.g. 1_000_000 and 0xFF_FF (Issue #17, @develop7)
- Identifier under caret now highlights all other occurrences in the file, navigable via Edit > Find Usages > Next/Previous Highlighted Usage (Issue #18, @develop7)
- Structure view shows expandable function clauses for multi-equation definitions instead of just one entry (Issue #5, @develop7, @mirovarga)
- Extend Selection (Ctrl+W) now steps through individual do-notation binds, let-without-in blocks, and mdo blocks (Issue #16, @develop7)
- Code lenses no longer show raw JSON; unresolved lenses are now properly resolved via codeLens/resolve

### Thanks

- @develop7 for reporting all four issues
- @mirovarga for the structure view function clause request

## [2026.1.11] - 2026-04-07

### Added

- Extend Selection (Ctrl+W) now expands through parenthesized groups, function applications, if/case/let/do/lambda constructs, case alternatives, and function clause boundaries instead of jumping directly from a word to the entire function (Issue #16, @develop7)

### Thanks

- @develop7 for the detailed Extend Selection feature request with expected expansion steps

## [2026.1.10] - 2026-04-05

### Changed

- Improved syntax highlighting and fallback to language defaults (Issue #14, @wolverian)
- Semantic highlighting for function names, record fields, and exported names in module headers; colors match IntelliJ conventions

### Thanks

- @wolverian for requesting Language Defaults support

## [2026.1.9] - 2026-04-02

### Fixed

- HLint fix suggestions (e.g. "Apply hint", "Ignore hint") now appear in Alt+Enter by passing diagnostics context to `textDocument/codeAction` (Issue #12, @wolverian)
- Fixed intermittent read-access exception when switching between Haskell files, caused by `ensureFileOpened` reading document content outside a read action (Issue #13, @wolverian)
- "Add type signature (HLS)" no longer appears spuriously on call sites; it now only activates when the cursor is on a binding definition (Issue #15, @wolverian)
- Fixed crash in "Add type signature" inspection quick fix when generating intention preview (F1), caused by null virtual file on preview copy

### Thanks

- @wolverian for follow-up feedback confirming the missing HLint actions, and reporting the file-switching exception and spurious type signature action

## [2026.1.8] - 2026-04-02

### Fixed

- Environment widget no longer runs `ghc --version` on the EDT, fixing the "Synchronous execution on EDT" error on project open (Issue #11, @wolverian)
- GHCup detection in "Install HLS" action no longer runs process calls on the EDT
- Code lens provider no longer throws `IllegalArgumentException` for null `unit` parameter, or `RuntimeExceptionWithAttachments` for PSI/document access outside a read action, on editor open
- HLS code actions now appear in Alt+Enter and execute correctly; code action fetching moved off EDT, lazy actions resolved via `codeAction/resolve`, and HLint fix suggestions now appear by passing diagnostics context to `textDocument/codeAction` (Issue #12, @wolverian)

### Thanks

- @wolverian for reporting the EDT violation on project open and non-functional HLS code actions

## [2026.1.7] - 2026-04-01

### Fixed

- False positive "duplicate import" when same module is imported both qualified and unqualified (Issue #10, @develop7)
- Structure view now shows function definitions instead of type signatures, fixing cases where functions were missing or navigated to the wrong location (Issue #5, @develop7, @mirovarga)

### Thanks

- @develop7 for reporting both issues
- @mirovarga for also reporting the structure view issue

## [2026.1.6] - 2026-03-26

### Added

- Code actions from HLS via Alt+Enter: import missing symbols, apply HLint fixes, insert type signatures, and other refactors
- Auto-closing pairs for `{-# #-}` pragmas, `(# #)` unboxed tuples, and `[qq| |]` quasiquote brackets

- Inlay hints from HLS showing inferred types and parameter names inline (toggle in Settings > Editor > Inlay Hints)
- Code lenses showing inferred type signatures above bindings; click to insert
- Find Usages now queries HLS for cross-file references
- Four new intention actions: add type annotation, add LANGUAGE pragma, toggle explicit import list, add deriving clause
- HLint diagnostics now tagged with `[HLint: rule]` prefix in tooltip and rendered as weak warnings

### Thanks

- @develop7 for continued feedback on LSP feature parity

## [2026.1.5] - 2026-03-24

### Added

- Call hierarchy (Ctrl+Alt+H) — shows callers and callees of any function, data type, or class declaration (benjamin-thomas)

### Fixed

- Improved Psi-Tree structure

### Thanks

- @benjamin-thomas for suggesting call hierarchy support

## [2026.1.4] - 2026-03-22

### Added

- Improved validation with 4 new HLint-style inspections (unused LANGUAGE pragma, pragma syntax, camelCase, many imports), per-symbol unused import detection, and smarter HLS duplicate suppression
- Improved local on-hover documentation
- Added temporary language injection via ALT+Enter in Strings (Issue #9)

### Thanks

- @dnikolovv for requesting language injection support in string literals (Issue #9)

## [2026.1.3] - 2026-03-21

### Added

- Ctrl+click on import module names and symbols now navigates to the source file without requiring HLS (@dnikolovv)
- Improved formatter settings (File - Settings - Languages & Frameworks - Haskell)
- HLS settings: on/off toggle, per-feature toggles, analysis mode (Enhanced or Server Only)
- .cabal and cabal.project file support with syntax highlighting, completion, and cabal-fmt formatting (Issue #8, @mirovarga)

### Fixed

- Ctrl+click go-to-definition now handles both LSP Location and LocationLink response formats (Issue #8, @dnikolovv)
- Rename applies workspace edits across all affected files
- Hover no longer blocks the UI thread
- Fixed false positive "missing type signature" warnings near deriving declarations (Issue #6, @mirovarga)
- Disabled "Missing module header" inspection by default

### Thanks

- @dnikolovv for requesting external library navigation (Issue #8)
- @mirovarga for various bug reports and requesting .cabal file support (Issue #6, Issue #8)
- @develop7 for his continuing support and exchange of ideas (that's awesome!)

## [2026.1.2] - 2026-03-18

### Added

- Reworked the on-hover documentation (Issue #7, @mirovarga)
- Various grammar upgrades

### Fixed

- QuasiQuote injector: added missing quoters including `[i|...|]` for ihamlet (Issue #2, @develop7)
- Formatter: built-in Reformat Code no longer destroys indentation and qualified imports (Issue #4, @mirovarga)
- Structure panel: removed fake expand arrows, deduplicated entries, imports now visible (Issue #5, @mirovarga)
- Inspections: disabled broken inspections that flagged non-existing problems (Issue #6, @mirovarga)
- Parser: `where`-blocks no longer split into orphan nodes; indentation preserved after Ctrl+Alt+L on class/instance/function declarations
- Settings: left-side section navigation, resizable description panel with persistent size

### Thanks

- @develop7 for reporting missing QuasiQuote quoter `[i|...|]` (Issue #2)
- @mirovarga for reporting formatter, structure panel, inspection, and hover documentation issues (Issues #4, #5, #6, #7)

## [2026.1.1] - 2026-03-18

### Added

- QuasiQuote support: lexing, language injection (sql, hamlet, julius, cassius, etc.), qualified quoter names (Issue #2, @develop7)

### Fixed

- HLS on macOS/Linux: `file:///` URIs, READY transition, and `didOpen` lifecycle (Issue #1, @mirovarga)
- SDK: registration crash and NPE during crash recovery (@dnikolovv)
- GHCi frozen after stop; wrong URI in rename/go-to-definition on Unix; off-EDT and duplicate `didOpen` race (Issue #3, @mirovarga)

### Thanks

- @mirovarga for reporting HLS stuck at Initializing and GHCi frozen after stop (Issues #1, #3)
- @dnikolovv for reporting the SDK registration crash and NPE in crash recovery
- @develop7 for reporting QuasiQuotes lack basic highlighting (Issue #2)

## [2026.1.0]

### Added

- Haskell language support with syntax highlighting
- HLS (Haskell Language Server) integration
- GHCi REPL terminal integration
- Stack/Cabal/GHC build tool support

[Unreleased]: https://gitlab.ilscipio.com/ilscipio/projects/flexible-haskell/compare/v2026.2.0...HEAD
[2026.2.0]: https://gitlab.ilscipio.com/ilscipio/projects/flexible-haskell/compare/v2026.1.60...v2026.2.0
[2026.1.60]: https://gitlab.ilscipio.com/ilscipio/projects/flexible-haskell/compare/v2026.1.59...v2026.1.60
[2026.1.59]: https://gitlab.ilscipio.com/ilscipio/projects/flexible-haskell/compare/v2026.1.58...v2026.1.59
[2026.1.58]: https://gitlab.ilscipio.com/ilscipio/projects/flexible-haskell/compare/v2026.1.57...v2026.1.58
[2026.1.57]: https://gitlab.ilscipio.com/ilscipio/projects/flexible-haskell/compare/v2026.1.56...v2026.1.57
[2026.1.56]: https://gitlab.ilscipio.com/ilscipio/projects/flexible-haskell/compare/v2026.1.55...v2026.1.56
[2026.1.55]: https://gitlab.ilscipio.com/ilscipio/projects/flexible-haskell/compare/v2026.1.54...v2026.1.55
[2026.1.54]: https://gitlab.ilscipio.com/ilscipio/projects/flexible-haskell/compare/v2026.1.53...v2026.1.54
[2026.1.53]: https://gitlab.ilscipio.com/ilscipio/projects/flexible-haskell/compare/v2026.1.52...v2026.1.53
[2026.1.52]: https://gitlab.ilscipio.com/ilscipio/projects/flexible-haskell/compare/v2026.1.51...v2026.1.52
[2026.1.51]: https://gitlab.ilscipio.com/ilscipio/projects/flexible-haskell/compare/v2026.1.50...v2026.1.51
[2026.1.50]: https://gitlab.ilscipio.com/ilscipio/projects/flexible-haskell/compare/v2026.1.49...v2026.1.50
[2026.1.49]: https://gitlab.ilscipio.com/ilscipio/projects/flexible-haskell/compare/v2026.1.48...v2026.1.49
[2026.1.48]: https://gitlab.ilscipio.com/ilscipio/projects/flexible-haskell/compare/v2026.1.47...v2026.1.48
[2026.1.47]: https://gitlab.ilscipio.com/ilscipio/projects/flexible-haskell/compare/v2026.1.46...v2026.1.47
[2026.1.46]: https://gitlab.ilscipio.com/ilscipio/projects/flexible-haskell/compare/v2026.1.45...v2026.1.46
[2026.1.45]: https://gitlab.ilscipio.com/ilscipio/projects/flexible-haskell/compare/v2026.1.44...v2026.1.45
[2026.1.44]: https://gitlab.ilscipio.com/ilscipio/projects/flexible-haskell/compare/v2026.1.43...v2026.1.44
[2026.1.43]: https://gitlab.ilscipio.com/ilscipio/projects/flexible-haskell/compare/v2026.1.42...v2026.1.43
[2026.1.42]: https://gitlab.ilscipio.com/ilscipio/projects/flexible-haskell/compare/v2026.1.41...v2026.1.42
[2026.1.41]: https://gitlab.ilscipio.com/ilscipio/projects/flexible-haskell/compare/v2026.1.40...v2026.1.41
[2026.1.40]: https://gitlab.ilscipio.com/ilscipio/projects/flexible-haskell/compare/v2026.1.39...v2026.1.40
[2026.1.39]: https://gitlab.ilscipio.com/ilscipio/projects/flexible-haskell/compare/v2026.1.38...v2026.1.39
[2026.1.38]: https://gitlab.ilscipio.com/ilscipio/projects/flexible-haskell/compare/v2026.1.37...v2026.1.38
[2026.1.37]: https://gitlab.ilscipio.com/ilscipio/projects/flexible-haskell/compare/v2026.1.36...v2026.1.37
[2026.1.36]: https://gitlab.ilscipio.com/ilscipio/projects/flexible-haskell/compare/v2026.1.35...v2026.1.36
[2026.1.35]: https://gitlab.ilscipio.com/ilscipio/projects/flexible-haskell/compare/v2026.1.34...v2026.1.35
[2026.1.34]: https://gitlab.ilscipio.com/ilscipio/projects/flexible-haskell/compare/v2026.1.33...v2026.1.34
[2026.1.33]: https://gitlab.ilscipio.com/ilscipio/projects/flexible-haskell/compare/v2026.1.32...v2026.1.33
[2026.1.32]: https://gitlab.ilscipio.com/ilscipio/projects/flexible-haskell/compare/v2026.1.31...v2026.1.32
[2026.1.31]: https://gitlab.ilscipio.com/ilscipio/projects/flexible-haskell/compare/v2026.1.30...v2026.1.31
[2026.1.30]: https://gitlab.ilscipio.com/ilscipio/projects/flexible-haskell/compare/v2026.1.29...v2026.1.30
[2026.1.29]: https://gitlab.ilscipio.com/ilscipio/projects/flexible-haskell/compare/v2026.1.28...v2026.1.29
[2026.1.28]: https://gitlab.ilscipio.com/ilscipio/projects/flexible-haskell/compare/v2026.1.27...v2026.1.28
[2026.1.27]: https://gitlab.ilscipio.com/ilscipio/projects/flexible-haskell/compare/v2026.1.26...v2026.1.27
[2026.1.26]: https://gitlab.ilscipio.com/ilscipio/projects/flexible-haskell/compare/v2026.1.25...v2026.1.26
[2026.1.25]: https://gitlab.ilscipio.com/ilscipio/projects/flexible-haskell/compare/v2026.1.24...v2026.1.25
[2026.1.24]: https://gitlab.ilscipio.com/ilscipio/projects/flexible-haskell/compare/v2026.1.23...v2026.1.24
[2026.1.23]: https://gitlab.ilscipio.com/ilscipio/projects/flexible-haskell/compare/v2026.1.22...v2026.1.23
[2026.1.22]: https://gitlab.ilscipio.com/ilscipio/projects/flexible-haskell/compare/v2026.1.21...v2026.1.22
[2026.1.21]: https://gitlab.ilscipio.com/ilscipio/projects/flexible-haskell/compare/v2026.1.20...v2026.1.21
[2026.1.20]: https://gitlab.ilscipio.com/ilscipio/projects/flexible-haskell/compare/v2026.1.19...v2026.1.20
[2026.1.19]: https://gitlab.ilscipio.com/ilscipio/projects/flexible-haskell/compare/v2026.1.18...v2026.1.19
[2026.1.18]: https://gitlab.ilscipio.com/ilscipio/projects/flexible-haskell/compare/v2026.1.17...v2026.1.18
[2026.1.17]: https://gitlab.ilscipio.com/ilscipio/projects/flexible-haskell/compare/v2026.1.16...v2026.1.17
[2026.1.16]: https://gitlab.ilscipio.com/ilscipio/projects/flexible-haskell/compare/v2026.1.15...v2026.1.16
[2026.1.15]: https://gitlab.ilscipio.com/ilscipio/projects/flexible-haskell/compare/v2026.1.14...v2026.1.15
[2026.1.14]: https://gitlab.ilscipio.com/ilscipio/projects/flexible-haskell/compare/v2026.1.13...v2026.1.14
[2026.1.13]: https://gitlab.ilscipio.com/ilscipio/projects/flexible-haskell/compare/v2026.1.12...v2026.1.13
[2026.1.12]: https://gitlab.ilscipio.com/ilscipio/projects/flexible-haskell/compare/v2026.1.11...v2026.1.12
[2026.1.11]: https://gitlab.ilscipio.com/ilscipio/projects/flexible-haskell/compare/v2026.1.10...v2026.1.11
[2026.1.10]: https://gitlab.ilscipio.com/ilscipio/projects/flexible-haskell/compare/v2026.1.9...v2026.1.10
[2026.1.9]: https://gitlab.ilscipio.com/ilscipio/projects/flexible-haskell/compare/v2026.1.8...v2026.1.9
[2026.1.8]: https://gitlab.ilscipio.com/ilscipio/projects/flexible-haskell/compare/v2026.1.7...v2026.1.8
[2026.1.7]: https://gitlab.ilscipio.com/ilscipio/projects/flexible-haskell/compare/v2026.1.6...v2026.1.7
[2026.1.6]: https://gitlab.ilscipio.com/ilscipio/projects/flexible-haskell/compare/v2026.1.5...v2026.1.6
[2026.1.5]: https://gitlab.ilscipio.com/ilscipio/projects/flexible-haskell/compare/v2026.1.4...v2026.1.5
[2026.1.4]: https://gitlab.ilscipio.com/ilscipio/projects/flexible-haskell/compare/v2026.1.3...v2026.1.4
[2026.1.3]: https://gitlab.ilscipio.com/ilscipio/projects/flexible-haskell/compare/v2026.1.2...v2026.1.3
[2026.1.2]: https://gitlab.ilscipio.com/ilscipio/projects/flexible-haskell/compare/v2026.1.1...v2026.1.2
[2026.1.1]: https://gitlab.ilscipio.com/ilscipio/projects/flexible-haskell/compare/v2026.1.0...v2026.1.1
[2026.1.0]: https://gitlab.ilscipio.com/ilscipio/projects/flexible-haskell/commits/v2026.1.0
