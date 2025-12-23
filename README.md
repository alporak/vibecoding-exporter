# vibecoding-exporter ⚡

**vibecoding-exporter** is a "surgical" code dumping tool designed to pack large C/C++ projects into tiny AI context windows. 

Instead of dumping entire source files and wasting thousands of tokens on unused code and comments, this tool performs symbol-level dependency analysis. It identifies the functions actually reachable from your entry point and exports only what the AI needs to see.

## 🚀 Why use this?

- **Token Efficiency:** Automatically strips all C-style comments and collapses whitespace.
- **Surgical Extraction:** If your `utils.c` has 100 functions but your `main.c` only calls 2, only those 2 functions are exported.
- **Recursive Resolution:** It follows the "logic tree." If `main()` calls `foo()`, and `foo()` calls `bar()`, it will find and include `bar()` even if it's three files deep.
- **Vibe-Checked:** Built specifically for pasting code into LLMs (Claude 3.5 Sonnet, GPT-4o) where context window space is premium.

## 🛠 Features

- **Symbol Analysis:** Parses C/C++ to identify function bodies, structs, typedefs, and macros.
- **Iterative Discovery:** Continuously scans extracted code to find nested dependencies.
- **Context Preservation:** Keeps all `#define` and `struct` definitions by default, as they are high-value/low-token context.
- **Crash-Proof Config:** Remembers your entry point, search depth, and output preferences in a `.project_dumper_config.json`.

## 📦 Installation

No dependencies required. Just Python 3.x.

```bash
git clone https://github.com/your-username/vibecoding-exporter.git
cd vibecoding-exporter
```

## 📖 Usage

Run the script from your project root:

```bash
python file_dump.py
```

### Prompted Options:
1. **Entry file:** The starting point of your logic (e.g., `src/main.c` or `src/fota_handler.c`).
2. **Search depth:** How many layers of `#include` files to follow (Default: `3`).
3. **Output file:** Where to save the compressed dump (Default: `project_dump.txt`).

## 📄 Output Format

The exporter produces a clean, dense text file:

```c
// --- FILE: src/main.c ---
#include "fota.h"
#define VERSION 1
void main() {
    run_fota_update();
}

// --- FILE: src/fota.c ---
/* Only the used function is extracted. Unused functions in this file are ignored. */
void run_fota_update() {
    // Logic here...
}
```

## ⚖️ License
MIT
