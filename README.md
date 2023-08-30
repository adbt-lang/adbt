# 📄 ADBT 🪅

🐲 This file contains the latest (**`v2.0.0`**) specification and documentation of `ADBT` language and its source, Adblock template files ⚡

<br>

> 🚀 Head to the [Releases](https://github.com/igorskyflyer/file-format-adbt/releases/) page to read the specifications of all revisions.

<br>

## 📃 Table of Contents

- [Introduction](#-introduction)
- [Motivation](#-motivation)
- [Quick start](#-quick-start)
- [File information](#-file-information)
- [File structure](#%EF%B8%8F-file-structure)
- [Syntax](#-syntax)
  - [Statements](#-statements)
    - [header](#header)
    - [include](#include)
    - [import](#import)
    - [tag](#tag)
    - [export](#export)
    - [nl](#nl)
  - [Ordering](#-ordering)
  - [Actions](#-actions)
  - [Strings](#%EF%B8%8F-strings)
  - [Paths](#%EF%B8%8F-paths)
  - [Comments](#-comments)
    - [Internal](#internal)
    - [Exported](#exported)
- [Inline meta](#-inline-meta)
- [Meta files](#-meta-files)
- [Variables](#-variables)
  - [Meta variables](#-meta-variables)
    - [Example](#-example)
  - [Compile variables](#%EF%B8%8F-compile-variables)
    - [Example](#example)
- [Samples](#-samples)
- [License](#-license)
- [Related](#-related)

---

## 📍 Introduction

`ADBT` files are template files that provide ways of writing reusable, component-like based Adblock filter files.

`ADBT` are text-based, UTF-8 encoded files and use LFs for line-breaks.

> 💡 Even if you add CRLFs, the compiler will auto-convert them to LFs and the output file will contain only LFs.

`ADBT` templates contain header and filter rule files that will get compiled to a single filter list file.

`ADBT` files and their compiler `Aria` are Adblock syntax-agnostic, you are free to use any Adblock syntax.

`ADBT` files can work in conjunction with optional, complimentary files `*.adbm`.

---

## 📌 Motivation

Ad-blocking filters have always been a valuable tool to improve web browsing experiences but creating and maintaining them often poses challenges. The existing methods can be cumbersome, leading to repetitive tasks and difficult-to-manage source code.  
Recognizing this issue, I embarked on a project to develop a custom language, file format, parser and a Visual Studio Code extension tailored specifically as Adblock templates. My motivation is to offer filter lists creators and maintainers a more straightforward and efficient way to craft filter lists.

By using `ADBT`, maintainers will be able to incorporate multiple rules into different output filter lists, have the versioning of filter lists done automatically for them, include both internal and exported comments, organize filter lists and filter rules.

Ultimately, my goal is to empower the Adblock community with an intuitive and user-friendly solution, enhancing the overall ad-blocking experience for everyone.

`ADBT` and its toolchain have also been created to ease the maintenance of my Adblock filter list, [AdVoid](https://github.com/igorskyflyer/ad-void) and `AdVoid` is the first filter list that uses `ADBT` under the hood.

---

## 🍭 Quick start

Even though technically you can write `ADBT` \(`*.adbt`) templates and meta files \(`*.adbm`) in any text editor, I highly recommend using [Visual Studio Code](https://code.visualstudio.com/) as your editor.

<br>

Creating and editing `*.adbt` templates and their complementary `*.adbm` meta files is available for Visual Studio Code via the [ADBT extension](https://marketplace.visualstudio.com/items?itemName=igordvlpr.adbt) I created and it includes the following features:

- high-performance due to small footprint,
- language support and encoding for `*.adbt` files,
- syntax highlighting,
- auto-complete (Intellisense):
  - functions/statements (including path placeholders),
  - comments (including comment modifiers, i.e. `TODO`, `FIXME`, `NOTE`),
  - directives,
- hover information,
- snippets,
- meta files `*.adbm` support, relies on built-in JSON support:
  - autocomplete (Intellisense),
  - hover info,
- custom file icon.

<br>

To actually compile the templates you need to install the current Node >= 18 (**LTS**) and the [`Aria compiler`](https://www.npmjs.com/package/@igor.dvlpr/aria).

Install Node by navigating to their [official site](https://nodejs.org/en).

Install `Aria` by executing any of the following:

Global install

```shell
npm i -g "@igor.dvlpr/aria"
```

<br>

Local install

```shell
npm i "@igor.dvlpr/aria"
```

---

## 🌈 File information

`ADBT` file information:

Type: text  
Extension: `.adbt`  
Encoding: UTF-8  
Syntax: custom  
Line break: LF

<br>

`ADBT meta` file information:

Type: text  
Extension: `.adbm`  
Encoding: UTF-8  
Syntax: JSON (custom schema)  
Line break: editor-dependent

> 💡 JSON files support both LFs and CRLFs

---

## ⚙️ File structure

`ADBT` files follow an exact order and rules of their source code, described below:

- any line can be blank and all whitespace will be ignored when compiling
  - an explicit blank line can be used as well,
- any line can contain a comment (either internal or exported)
  - comments are not allowed to exist on the same line with statements,
- all strings, including paths must be enclosed within single quotes
  - in case of a single quote inside a string, it must be escaped, see [Strings](#EF%B8%8F-strings) for more information
- header files can be included multiple times
  - preferably, you should only have 1 header file and include it,
- filter files can be included multiple times,
- export statement **must** be the last statement of the file
  - only 1 export statement is allowed per template file,
  - any code after the first `export` statement will be considered unreachable and it won't be parsed nor compiled

<br>

❗Each `ADBT` template can be compiled to only 1 filter list file.

---

## 🧪 Syntax

### ⚡ Statements

#### `header`

> Imports a header file.

<br>

Accepts: `path: string`

<br>

Example:

`template.adbt`

```shell
header './headers/my-header.txt'
```

<br>

Header files are plain text files and although not mandatory, they usually end with a `.txt` extension.

<br>

Adblock header files should contain the metadata that will be used for the resulting Adblock filter file.

<br>

> 💡 Metadata is composed of special comments that describe your filter list closely, like the filter list title, number of entries, version, modified date, author, etc.

<br>

Here's an example:

`header.txt`

```adblock
[Adblock Plus 2.0]
! Title: AdVoid.Core
! Description: ✈ AdVoid is an efficient AdBlock filter that blocks ads, trackers, malware and a lot more if you want it to! 👾
! Version: 1.8.1082
! Last modified: 2023-07-23T19:53:00+0200
! Expires: 6 hours (update frequency)
! Homepage: https://github.com/igorskyflyer/ad-void
! Entries: 2533
! Author: Igor Dimitrijević (@igorskyflyer)
```

The `header` should be at the top of the `ADBT` template file; comments are allowed before it.

❗Path to the header file to include can be either relative or absolute but must be enclosed within single quotes. Failing to do so, will produce a fatal error. If a single or multiple single quotes are present in the path, it/they must be escaped with the escape sequence, a backslash followed by a single quote, i.e. `\'`.

<br>

#### `include`

> Includes an Adblock filter list file.

<br>

Accepts: `path: string`

<br>

Example:

`template.adbt`

```shell
include './rules/domains.txt'
include './rules/cosmetic.txt'
include './rules/query.txt'
```

<br>

Filter list files are plain text files and although not mandatory, they usually end with a `.txt` extension.

The filter list file should contain **only** filter rules and Adblock comments, i.e.:

`rules.txt`

```adblock
||somesite.abc^
||somesite.abc^
||somesite.abc^
||somesite.abc^
||somesite.abc^
##.someclass
##.someclass
! comment
##.someclass
##.someclass
##.someclass
```

It should **not** include any metadata - that should be included via the [`header`](#header) statement. Doing otherwise will result in metadata conflicts.
It can contain any valid filter rules and comments.

❗Path to the filter list file to include can be either relative or absolute but must be enclosed within single quotes. Failing to do so, will produce a fatal error. If a single or multiple single quotes are present in the path, it/they must be escaped with the escape sequence, a backslash followed by a single quote, i.e. `\'`.

<br>

#### `import`

> Includes an Adblock filter list file.

<br>

Accepts: `path: string`

<br>

Example:

`template.adbt`

```shell
import './rules/domains.txt'

nl

import './rules/cosmetic.txt'

nl

import './rules/query.txt'

export '.my-filter.txt'
```

<br>

> 💡 **`import`** statements behave exactly the same as **[`include`](#include)** statements but will prepend the file path of the included filter (as a comment).

<br>

<br>

`my-filter.txt` (_compiled_)

```adblock
! *** ./rules/domains.txt ***
||somedomain1.com^
||somedomain2.com^
||somedomain3.com^

! *** ./rules/cosmetic.txt ***
##.someclass1
##.someclass2

! *** ./rules/query.txt ***
query=
another-query=
```

<br>

❗Paths are prepended as-is, no matter if the path is relative/absolute.

<br>

#### `tag`

> Inserts a tag.

<br>

Accepts: `description: string` _(optional)_

<br>

Example:

`template.adbt`

```shell
tag 'Block these domains'
include './rules/domains.txt'

tag 'Hide these elements'
include './rules/cosmetic.txt'

export 'my-filter.txt'
```

<br>

`my-filter.txt` (_compiled_)

```adblock
! {@0} Block these domains
||somedomain1.com^
||somedomain2.com^
||somedomain3.com^

! {@1} Hide these elements
##.someclass1
##.someclass2
```

<br>

🤔 What are tags exactly?

Tags are a part of the tagging system; special comments that get inserted in the resulting filter file, for easier navigation, search, etc.  
Tags are auto-enumerated - starting with `0` and can contain an optional description.

_🌟 Inspired by [AdVoid](https://github.com/igorskyflyer/ad-void)'s way of navigation._

<br>

#### `export`

> Exports the compiled filter list to a file.

<br>

Accepts: `path: string`

<br>

Example:

`template.adbt`

```shell
include './rules/domains.txt'
include './rules/cosmetic.txt'
include './rules/query.txt'

export './my-filter.txt'
```

<br>

`my-filter.txt` (_compiled_)

```adblock
||somedomain1.com^
||somedomain2.com^
||somedomain3.com^
##.someclass1
##.someclass2
query=
another-query=
```

❗Path of the file to export to must be enclosed within single quotes. Failing to do so, will produce a fatal error. If a single or multiple single quotes are present in the path, it/they must be escaped with the escape sequence, a backslash followed by a single quote, i.e. `\'`.

<br>

> 💡 Any code appearing after the first occurrence of an export statement will be considered unreachable and won't be parsed nor compiled.

<br>

#### `nl`

> Generates an explicit blank newline.

<br>

Accepts: N/A

<br>

Example:

`template.adbt`

```shell
nl
```

<br>

The newline will be present in the output/compiled filter file. Used to improve readability and/or organize your rules, see [samples](#-samples) below.

---

### 🦭 Ordering

`ADBT` templates must follow a certain structure, i.e., order of statements (which is enforced by the compiler).

The following rules are enforced:

- a [`header`](https://github.com/igorskyflyer/file-format-adbt#header) statement cannot appear after a [`meta`](https://github.com/igorskyflyer/file-format-adbt#meta) statement,
- a [`header`](https://github.com/igorskyflyer/file-format-adbt#header) statement cannot appear after an [`include`](https://github.com/igorskyflyer/file-format-adbt#include)/[`import`](https://github.com/igorskyflyer/file-format-adbt#import) statement,
- a [`meta`](https://github.com/igorskyflyer/file-format-adbt#meta) statement cannot appear after an [`include`](https://github.com/igorskyflyer/file-format-adbt#include)/[`import`](https://github.com/igorskyflyer/file-format-adbt#import) statement,
- **`no statements`** can appear after an [`export`](https://github.com/igorskyflyer/file-format-adbt#export) statement.

---

### 🥊 Actions

Actions allow you to invoke a certain function when executing a certain statement.

Two statements are currently supported:

- [**`include`**](#include),
- [**`import`**](#import)

<br>

#### 🦴 Supported actions

**`trim`**  
Trims whitespace for each line from the included filter list file.

<br>

`example.adbt`

```shell
include './my-list.txt' trim
import './my-list.txt'  trim
```

<br>

**`dedupe`**  
Removes duplicates from the included filter list file.

<br>

`example.adbt`

```shell
include './my-list.txt' dedupe
import './my-list.txt'  dedupe
```

<br>

**`sort`**  
Sorts lines from the included filter list file.

<br>

`example.adbt`

```shell
include './my-list.txt' sort=desc
import './my-list.txt'  sort=desc
```

Supports an optional param that controls whether sorting should be done in ascending or descending order.

To sort in ascending order, pass `asc` as the param value.  
To sort in descending order, pass `desc` as the param value.

> 💡 If no param is passed, `asc` is inferred.

<br>

**`append`**  
Appends an arbitrary string to each line from the included filter list file.

<br>

`example.adbt`

```shell
include './my-list.txt' append='$third-party'
import './my-list.txt'  append='$third-party'
```

The required param must be a valid string.  
See [Strings](#%EF%B8%8F-strings) for more information.

<br>

**`strip`**  
Strips a certain element of each line from the included filter list file.

<br>

`example.adbt`

```shell
include './my-list.txt' strip=modifiers
import './my-list.txt'  strip=modifiers
```

Supported params are:

- **`modifiers`** - will strip out all modifiers, e.g. `$third-party`, `$script`, etc.
- **`comments`** - will strip out all comments.

---

### ✒️ Strings

Strings in `ADBT` are UTF-8 encoded and must be enclosed within single quotes. If a single or multiple single quotes are present in a string, it/they must be escaped with the escape sequence, a backslash followed by a single quote, i.e. `\'`.

---

### 🛣️ Paths

Paths in `ADBT` are UTF-8 encoded strings and must be enclosed within single quotes. If a single or multiple single quotes are present in a string, it/they must be escaped with the escape sequence, a backslash followed by a single quote, i.e. `\'`.

Paths can be either **`relative`** or **`absolute`**.

---

### 📢 Comments

`ADBT` files support two types of comments:

`internal` &ndash; comments that are only visible in the template file but **are not** exported to the compiled file,  
`exported` &ndash; comments that are visible in the template and **are** exported to the compiled file.

<br>

#### Internal

> Internal comments are prefixed by an `@` (at sign, asperand) and are line-based.

<br>

Example:

`template.adbt`

```shell
include './rules/domains.txt'
@ This is my internal comment
include './rules/cosmetic.txt'
@ This is my another internal comment
include './rules/query.txt'
```

<br>

#### Exported

> Exported comments are prefixed by an `#` (hash, pound, number sign) and are line-based.

<br>

Example:

`template.adbt`

```shell
include './rules/domains.txt'
# This will be present in the compiled file
include './rules/cosmetic.txt'
# This too!
include './rules/query.txt'
```

---

### ⚡ Inline meta

Inline meta is metadata defined in an `ADBT` template itself; as opposed to using external, `.adbm` meta files.
Inline meta allows the user to override both the header and external metadata since inline meta has the highest priority.

---

### 🔋 Meta files

`ADBT` template files can contain a header file - as mentioned [here](#header) but in real-life use case we usually want to have a single, reusable header file with its metadata.

Unfortunately, a common header file most certainly has a property that should be different for every filter list file we compiled, in most cases, at least the `! Title: Filter name` should be different for every filter list file.

This is where _meta files_ come into play. Meta files are complementary files with an extension of `*.adbm` that provide a way of having a dynamic header file that can be reused in all `ADBT` template files. We write placeholders in our common header file that get replaced at compile-time.

<br>

> ℹ️ Meta files are completely optional, use them only when needed.

<br>

Meta files have a special naming convention.
If you fail to name them properly they won't be recognized by the compiler.  
Meta files should be named after the `ADBT` template basename, see an example below.

<br>

`ADBT` template name: `my-template.adbt`  
`ADBT` meta file name: `my-template.adbm`

<br>

In a nutshell, meta files' filename should be:

> \<templateBasename\> + ".adbm"

Meta files have properties of their own, even though you are already familiar with its base, JSON (if not, you can learn more [here](https://www.json.org/json-en.html)).

👀 Continue reading to learn how to use meta file (and other) variables.

---

### 🔮 Variables

`ADBT` in conjunction with its compiler `Aria` provide a mechanism of using variables inside plain header/filter files in order to maximise efficiency, reusability and customizability.

<br>

There are 2 types of variables available:

- `meta variables`, template-based, local, stored in an `*.adbm` file,
- `compile variables`, compiler-based, global, available anywhere in the template file, computed during compile time.

---

#### 🦠 Meta variables

In its earliest stage, the current properties can be stored in an `*.adbm` file:

- `title`,
- `description`,
- `expires`,
- `versioning`

<br>

Each property in the `*.adbm` file has a corresponding placeholder variable that you can use in your header files. Placeholders can have aliases. Placeholders provided by meta files are substituted during compile-time.

Placeholders for meta variables have the following syntax `{placeholder}`.

<br>

##### Available meta variables

#### `title: string`

The title of the filter list.

Placeholder: `{title}`

<br>

#### `description: string`

The description of the filter list.

Placeholders: `{description}`, `{about}`

<br>

#### `expires: string`

The expires field of the filter list.

Placeholders: `{expires}`

<br>

#### `versioning: "auto" | "semver" | "timestamp"`

The versioning system of the filter list.

Placeholders: N/A

<br>

> ❗ Unlike the previous 2 variables, this is a compiler-behaviour variable only, it controls which versioning system to use for the exported filter list file, thus you cannot use it a header file for substitution.

> 💫 Versioning can also be set via the `--versioning` flag of the `Aria` compiler, see the [official documentation](https://github.com/igorskyflyer/npm-adblock-aria-compiler#versioning) of it.

<br>

Available options are:

- `auto`: **default**, let the compiler decide which versioning system to use. If the resulting file already exists, i.e. `Aria` already compiled the template before, it will re-use the versioning found in the file, otherwise it will use `semver`,
- `semver`: use valid SemVer versioning when exporting the filter file, e.g. v1.0.0, v2.199.222, etc.
  If no version is found the counting starts with v1.0.0,
- `timestamp`: use current UNIX timestamp, e.g. 1690409508.

<br>

#### 👀 Example

Here's an example of how to transform a common header file to a reusable, dynamic one:

<br>

`my-header.txt` (not reusable)

```adblock
[Adblock Plus 2.0]
! Title: AdVoid.Core
! Description: Will block trackers.
```

If we were to include this header file via the [`header`](#header) statement in multiple `ADBT` templates all of the resulting files would have the title of `AdVoid.Core` which is wrong because each filter list should have a unique name and (optionally) description and version.

Now, let's use meta files and their variables to modify our header file:

`my-header.txt` (**_reusable!_**)

```adblock
[Adblock Plus 2.0]
! Title: {title}
! Description: {about}
!             👆🏽 we could use {description} as well
```

Now, the header file has 2 dynamic properties, `title` and `description`. They are represented by the placeholders and are pulled from your meta file when the template is compiled, thus, allowing you to provide custom and different values in the meta file for each template.

<br>

Here is an example of:

- a reusable header file,
- a template file
  - its corresponding meta file,
- resulting, compiled filter list file

<br>

`my-header.txt`

```adblock
[Adblock Plus 2.0]
! Title: {title}
! Description: {description}
```

<br>

`popups.adbt`

```shell
header './headers/my-header.txt'

include './rules/popups.txt'
include './rules/annoyances.txt'
include './rules/sticky.txt'
@ more includes if needed

export './my-filter-list.txt'
```

<br>

`popups.adbm`

```json
{
  "title": "Pop Blocker",
  "description": "Will block popups."
}
```

<br>

`my-filter-list.txt` _(compiled)_

```adblock
[Adblock Plus 2.0]
! Title: Pop Blocker
! Description: Will block popups.
##.someclass
##.annoyance
##.someclass
##.xyz
##.someclass
```

---

#### 🗜️ Compile variables

Currently, 4 compile-time variables are available:

- `file`,
- `version`,
- `entries`,
- `lastModified`

<br>

Placeholders can have aliases.  
Placeholders are substituted during compile-time.  
Placeholders for compile variables have the following syntax `$placeholder`.

<br>

❗ Compile-time variable are computed when compilation is in progress.

<br>

#### Available compile variables

#### `file: string`

Filename of the current `ADBT` file. Useful for giving automatic titles to compiled filter files.

Placeholder: `$file`

<br>

##### Example

`my-header.txt`

```adblock
[Adblock Plus 2.0]
! Title: $file
```

<br>

`My Filter.adbt`

```shell
header './headers/my-header.txt'

include './rules/popups.txt'
include './rules/annoyances.txt'
include './rules/sticky.txt'
@ more includes if needed

export './my-filter-list.txt'

```

The compiled file `./my-filter-list.txt` will have the following metadata:

<br>

`my-filter-list.txt`

```adblock
[Adblock Plus 2.0]
! Title: My Filter
```

<br>

#### `version: string`

Current version of the compiled filter list file. The actual value depends on the selected [versioning system](#versioning).  
If one is not provided, the versioning system will be inferred.

Placeholders: `$version`, `$v`

<br>

#### `entries: number`

The number of rules in the exported filter list file.

Placeholders: `$entries`, `$count`

<br>

#### `lastModified: string`

Current date and time formatted as an [ISO 8601](https://www.iso.org/iso-8601-date-and-time-format.html) string.

Placeholders: `$date`, `$now`

---

## 💡 Samples

The following samples are available:

- [header template (static)](./samples/header-only-static.adbt)
- [header template (dynamic)](./samples/header-only-dynamic.adbt)
- [complete template](./samples/import-rules.adbt)

---

## 🪪 License

Licensed under the MIT license which is available here, [MIT license](https://github.com/igorskyflyer/file-formats/blob/main/adbt/LICENSE).

---

## 🧬 Related

[@igor.dvlpr/adblock-filter-counter](https://www.npmjs.com/package/@igor.dvlpr/adblock-filter-counter)

> _🐲 A dead simple npm module that counts Adblock filter rules.🦘_

[@igor.dvlpr/keppo](https://www.npmjs.com/package/@igor.dvlpr/keppo)

> _🎡 Parse, manage, compare and output SemVer-compatible version numbers. 🧮_

[@igor.dvlpr/normalized-string](https://www.npmjs.com/package/@igor.dvlpr/normalized-string)

> _💊 NormalizedString provides you with a String type with consistent line-endings, guaranteed. 📮_

[@igor.dvlpr/zing](https://www.npmjs.com/package/@igor.dvlpr/zing)

> _🐌 Zing is a C# style String formatter for JavaScript that empowers Strings with positional arguments. 🚀_

[AdVoid](https://github.com/igorskyflyer/ad-void)

> _✈ AdVoid is an efficient AdBlock filter that blocks ads, trackers, malware and a lot more if you want it to! 👾_
