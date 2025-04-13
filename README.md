# Compiler

**Compiler** is an internal tool designed to efficiently summarize project directories for analysis by AI services like Google AI Studio, Claude, or ChatGPT. By scanning a selected folder (while intelligently ignoring common dependency/build directories), it generates a single, concise `project_summary.json` file. This file includes the project's directory structure, metadata about each file (path, size, type), and short text snippets, along with an embedded prompt to guide the AI. This approach significantly reduces token count compared to uploading full file contents, making it ideal for providing context to AI models quickly and cost-effectively.

---

## Table of Contents

-   [Overview](#overview)
-   [Features](#features)
-   [How It Optimizes for AI](#how-it-optimizes-for-ai)
-   [Installation](#installation)
-   [Usage](#usage)
-   [Output File Structure](#output-file-structure)
-   [Disclaimer](#disclaimer)
-   [Contributing](#contributing)

---

## Overview

**Compiler** streamlines preparing project context for AI platforms. Instead of uploading numerous files or excessively large amounts of text, you select a source directory, and **Compiler** generates a single `project_summary.json`. This summary contains the essential structural information and file metadata needed for an AI to understand the project's layout and components, optimizing for limited context windows and token usage. It features a simple GUI for ease of use.

---

## Features

-   **GUI Interface:** Simple graphical interface for selecting directories and starting the process.
-   **Directory Scanning:** Processes a selected source project directory.
-   **Intelligent Ignoring:** Automatically skips common dependency, build, and VCS folders (e.g., `node_modules`, `.git`, `venv`, `build`, `dist`) to focus on relevant source code.
-   **Single Summary Output:** Generates **one** comprehensive `project_summary.json` file per run.
-   **Unique Output Folders:** Creates a uniquely named (timestamped) subfolder for each run's output within a selected base output directory.
-   **Structure Mapping:** Includes a nested JSON object representing the original directory structure.
-   **File Metadata Extraction:** Records relative path, size, extension, and inferred type (`text`/`binary`) for each scanned file.
-   **Text Snippets:** Includes the first ~15 lines (up to ~1000 characters) of text files as a content preview. Binary file content is *not* included.
-   **Embedded AI Prompt:** The output JSON contains a suggested prompt (`ai_prompt_suggestion`) to guide an AI on how to interpret the summary file effectively.
-   **Token Optimization:** Specifically designed to provide maximum context with minimum token usage for AI models.

---

## How It Optimizes for AI

Large Language Models have limitations on input size (context window/tokens). Uploading entire codebases, especially those with large assets or dependencies, quickly exceeds these limits or becomes costly. **Compiler** addresses this by:

1.  **Ignoring Non-Essential Folders:** Skips directories like `node_modules`, reducing noise.
2.  **Excluding Full Content:** Only includes metadata and short *snippets* of text files, drastically cutting down text volume. Binary content (like images) is identified by type but not included.
3.  **Providing Structure:** The explicit directory tree helps the AI understand file relationships without needing full paths repeated constantly.
4.  **Consolidating Output:** A single summary file is easier to manage and upload than potentially thousands of individual files.

---

## Installation

This repository may include two versions of the tool:

-   **Executable (`.exe`):** (If provided) A ready-to-run version for Windows users needing a quick start. No Python installation required.
-   **Python Source Code (`.py` file):** For users who want to run from source or modify the tool.

### Using the Executable

1.  Download the `.exe` file.
2.  Double-click to run it.

### Using the Python Source Code

1.  Ensure Python 3.x is installed on your system.
2.  **(Optional but Recommended)** Create and activate a virtual environment:
    ```bash
    python -m venv venv
    # Windows:
    venv\Scripts\activate
    # macOS/Linux:
    source venv/bin/activate
    ```
3.  **(Optional for Themed Look)** Install the `ttkthemes` library for a nicer GUI appearance (if the script includes the theme option):
    ```bash
    pip install ttkthemes
    ```
4.  Run the tool from the command line, replacing `your_script_name.py` with the actual filename:
    ```bash
    python your_script_name.py
    ```

---

## Usage

1.  **Launch the Tool:** Open the executable or run the Python script.
2.  **Select Source Directory:** Click "Browse..." next to "Source Directory" and choose the root folder of the project you want to summarize.
3.  **Select Base Output Directory:** Click "Browse..." next to "Base Output Dir" and choose the main folder where you want the output subfolders to be created (e.g., `Documents/Output`).
4.  **Start Compiling:** Click the "Start Compiling" button.
5.  **Processing:** The tool will:
    -   Create a unique timestamped subfolder inside your selected Base Output Directory (e.g., `Output/MyProject_20240515_103000`).
    -   Scan the Source Directory, skipping ignored folders.
    -   Collect metadata and text snippets for included files.
    -   Generate the `project_summary.json` file inside the unique output subfolder.
    -   Log progress in the status window.
6.  **Output:** Navigate to the newly created timestamped folder within your Base Output Directory. You will find the single `project_summary.json` file. Upload this file to your AI service.

---

## Output File Structure

The generated JSON file, typically named `project_summary.json`, contains these main keys:

-   `ai_prompt_suggestion`: (String) A recommended prompt for instructing an AI on how to use this file.
-   `metadata`: (Object) Information about the generation process (source path, output folder name, list of ignored directories, snippet settings, file counts).
-   `directory_structure`: (Object) A nested representation of the original project's directory tree (folders contain a `children` list). Each node has `name` and `type` (`directory` or `file`).
-   `file_details`: (Array) A list where each element is an object describing a single file found during the scan:
    -   `path`: (String) Relative path from the source root.
    -   `size`: (Integer) File size in bytes.
    -   `extension`: (String) Lowercase file extension (e.g., `.py`, `.js`, `.png`).
    -   `file_type`: (String) Typically 'text' or 'binary'.
    -   `snippet`: (String | null) A short text preview (first ~15 lines / ~1000 chars) for `text` files, or `null` for binary files or empty files.

---

## Disclaimer

**Compiler** is intended for internal use, demonstration, and prototyping purposes. It simplifies preparing project context for AI analysis but does not guarantee perfect representation or suitability for all AI tasks. Use it at your own risk. Functionality may change over time.

---

## Contributing

Contributions are welcome! If you have ideas, bug fixes, or improvements:

-   **Fork the Repository:** Create your own branch for modifications.
-   **Submit a Pull Request:** Describe your changes and their benefits in detail.
-   **Open an Issue:** Use the GitHub Issues page (replace `yourusername/Compiler` with the actual URL if applicable, otherwise remove the placeholder link) to suggest features or report bugs.

Thank you for helping improve **Compiler**!

---
