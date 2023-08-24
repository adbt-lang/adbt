# 📄 ADBT 🪅

<br>

### v1.3.0

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

- **🪅 feat**: implement the **[`import`](./README.md#import)** statement
  > **`import`** statements behave exactly the same as **`include`** but prepend the file path of the included filter (as a comment)
- **🪅 feat**: implement the **[`tag`](./README.md#tag)** statement
  > Introduce a tagging system; special comments that get inserted in the resulting filter file, for easier navigation, search, etc.
  >
  > _🌟 Inspired by [AdVoid](https://github.com/igorskyflyer/ad-void)'s way of navigation._

<br>

### v1.1.0

- **🪅 feat**: add support for Expires meta variable
- **✅ fix**: ignore whitespace as a candidate for unreachable code
- **📜 docs**: add [Paths](./README.md#%EF%B8%8F-paths) section
- **📜 docs**: fix typos
- **📜 docs**: remove redundant data for `ADBT` extension for VS Code

<br>

### v1.0.1

- **📜 docs**: add missing export statement
- **📜 docs**: add unreachable code explanation
- **📜 docs**: add 1 export per template explanation
- **📜 docs**: fix headings
- **📜 docs**: add samples
