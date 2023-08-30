# 📄 ADBT 🪅

<br>

## v2.0.0

<p align="right"><em>31-Aug-2023</em></p>

- **🪅 feat** **\[BREAKING]**: enforce order of statements ([#60](https://github.com/igorskyflyer/npm-adblock-aria-compiler/issues/60))

> 💡 Keeps track of order of statements (nodes) in the input template and enforces rules, thus keeping the integrity and validity of the exported Adblock filter file.

The following rules are enforced:

- a [`header`](https://github.com/igorskyflyer/file-format-adbt#header) statement cannot appear after a [`meta`](https://github.com/metaigorskyflyer/file-format-adbt#meta) statement,
- a [`header`](https://github.com/igorskyflyer/file-format-adbt#header) statement cannot appear after an [`include`](https://github.com/igorskyflyer/file-format-adbt#include)/[`import`](https://github.com/igorskyflyer/file-format-adbt#import) statement,
- a [`meta`](https://github.com/igorskyflyer/file-format-adbt#meta) statement cannot appear after an [`include`](https://github.com/igorskyflyer/file-format-adbt#include)/[`import`](https://github.com/igorskyflyer/file-format-adbt#import) statement,
- **`no statements`** can appear after an [`export`](https://github.com/igorskyflyer/file-format-adbt#export) statement.

  Will `throw` when order is not correct.

- **🪅 feat**: add info logging method ([#86](https://github.com/igorskyflyer/npm-adblock-aria-compiler/issues/86))
- **🪅 feat**: log presence of inline meta ([#84](https://github.com/igorskyflyer/npm-adblock-aria-compiler/issues/84))
- **🪅 feat**: validate statements, catch edge-cases ([#82](https://github.com/igorskyflyer/npm-adblock-aria-compiler/issues/82))
- **🪅 feat**: detect and warn when no header/metadata is present ([#80](https://github.com/igorskyflyer/npm-adblock-aria-compiler/issues/80))
- **🪅 feat**: evaluate statements eagerly ([#74](https://github.com/igorskyflyer/npm-adblock-aria-compiler/issues/74))
- **🪅 feat**: reorganize order of nodes detection by usage/order of statements ([#72](https://github.com/igorskyflyer/npm-adblock-aria-compiler/issues/72))
- **🪅 feat**: add [`meta`](https://github.com/metaigorskyflyer/file-format-adbt#meta) statement and support for inline meta ([#68](https://github.com/igorskyflyer/npm-adblock-aria-compiler/issues/68))
  > 💡 Has highest priority when setting metadata.
- **🪅 feat**: always amend the `Expires` field of the metadata of the compiled filter file ([#66](https://github.com/igorskyflyer/npm-adblock-aria-compiler/issues/66))
- **🪅 feat**: always add `Entries` field to the metadata of the compiled filter file ([#64](https://github.com/igorskyflyer/npm-adblock-aria-compiler/issues/64))
- **🪅 feat**: make all user input paths universal, i.e. allow all OS' to use a forward slash (_"`/`"_) as the path separator ([#62](https://github.com/igorskyflyer/npm-adblock-aria-compiler/issues/62))

  > _🌟 Via [uPath](https://www.npmjs.com/package/@igor.dvlpr/upath)_

- **🪅 feat**: detect unsupported identifiers/code ([#58](https://github.com/igorskyflyer/npm-adblock-aria-compiler/issues/58))
- **✅ fix**: change warning text background color
- **✅ fix**: log unsupported identifiers ([#76](https://github.com/igorskyflyer/npm-adblock-aria-compiler/issues/76))
- **✅ fix**: refactor nodes logging ([#70](https://github.com/igorskyflyer/npm-adblock-aria-compiler/issues/70))
- **✅ fix**: import paths not being tracked ([#56](https://github.com/igorskyflyer/npm-adblock-aria-compiler/issues/56))
- **✅ fix**: actions remove final newline ([#54](https://github.com/igorskyflyer/npm-adblock-aria-compiler/issues/54))
- **✅ fix**: filter path not available in logs ([#52](https://github.com/igorskyflyer/npm-adblock-aria-compiler/issues/52))
- **✅ fix**: fix messages formatting ([#88](https://github.com/igorskyflyer/npm-adblock-aria-compiler/issues/88))
- **✅ fix**: various fixes to strings used in logging
- **💻 dev**: invert node orders ([#78](https://github.com/igorskyflyer/npm-adblock-aria-compiler/issues/78))
- **💻 dev**: externalize strings ([#90](https://github.com/igorskyflyer/npm-adblock-aria-compiler/issues/90))
- **💻 dev**: add tests and coverage

<br>

## v1.3.0

<p align="right"><em>25-Aug-2023</em></p>

- **🪅 feat**: add support for statement actions, currently available for:
  - [**`include`**](https://github.com/igorskyflyer/file-format-adbt#include),
  - [**`import`**](https://github.com/igorskyflyer/file-format-adbt#import)

<br>

> 💡Actions allow you to invoke a certain function when including/importing filter list files.
>
> Supported actions:
>
> - trim (trims whitespace for each line from the included filter list file)
> - dedupe (removes duplicates from the included filter list file)
> - sort (sorts lines from the included filter list file)
> - append (appends an arbitrary string to each line from the included filter list file)
> - strip (strips a certain element of each line from the included filter list file)

<br>

> You can read more about [Actions](https://github.com/igorskyflyer/file-format-adbt#-actions) in the official ADBT documentation.

- **📜 docs**: fixed typos

<br>

### v1.2.0

<p align="right"><em>20-Aug-2023</em></p>

- **🪅 feat**: implement the **[`import`](./README.md#import)** statement
  > **`import`** statements behave exactly the same as **`include`** but prepend the file path of the included filter (as a comment)
- **🪅 feat**: implement the **[`tag`](./README.md#tag)** statement
  > Introduce a tagging system; special comments that get inserted in the resulting filter file, for easier navigation, search, etc.
  >
  > _🌟 Inspired by [AdVoid](https://github.com/igorskyflyer/ad-void)'s way of navigation._

<br>

### v1.1.0

<p align="right"><em>19-Aug-2023</em></p>

- **🪅 feat**: add support for Expires meta variable
- **✅ fix**: ignore whitespace as a candidate for unreachable code
- **📜 docs**: add [Paths](./README.md#%EF%B8%8F-paths) section
- **📜 docs**: fix typos
- **📜 docs**: remove redundant data for `ADBT` extension for VS Code

<br>

### v1.0.1

<p align="right"><em>01-Aug-2023</em></p>

- **📜 docs**: add missing export statement
- **📜 docs**: add unreachable code explanation
- **📜 docs**: add 1 export per template explanation
- **📜 docs**: fix headings
- **📜 docs**: add samples

<br>

### v1.0.0

- **🚀 launch**: initial release

<p align="right"><em>31-Jul-2023</em></p>
