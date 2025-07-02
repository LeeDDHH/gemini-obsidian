# Gemini Obsidian Knowledge Management Vault

This repository contains an Obsidian vault integrated with a custom Gemini CLI, designed to streamline personal knowledge management following principles inspired by the Zettelkasten method. It leverages the power of Obsidian for note-taking and organization, enhanced by automated workflows via the Gemini CLI for processing information and generating insights.

## Features

This vault is equipped with custom Gemini CLI workflows to automate and enhance your knowledge management process:

-   **`/workflow:create_literature_note_from_daily`**: Automatically extracts URLs from your Daily Notes, fetches web page content, summarizes it using AI, creates new Literature Notes, and cleans up the processed URLs from the Daily Note.
-   **`/workflow:draft_permanent_note`**: Analyzes your Fleeting Notes and Literature Notes to identify related themes and ideas, helps you select a theme, and drafts a Permanent Note with links to relevant notes and thought-provoking questions.
-   **`/workflow:update_index_note`**: Keeps your knowledge base organized by automatically adding links to newly created Permanent Notes in your main Index Note.

## Directory Structure

-   `Daily/`: Your daily notes, often containing initial thoughts and captured URLs.
-   `FleetingNote/`: Quick, unrefined notes capturing immediate ideas.
-   `LiteratureNote/`: Summaries and insights from external sources (books, articles, web pages).
-   `PermanentNote/`: Refined, atomic notes representing core concepts and ideas.
-   `IndexNote/`: Maps and indexes to navigate your Permanent Notes and knowledge base.
-   `.gemini/`: Configuration files for the Gemini CLI.
-   `.obsidian/`: Obsidian's configuration files.

## Getting Started

### Prerequisites

-   [Obsidian](https://obsidian.md/) (Desktop application)
-   Gemini CLI (This project assumes you have the Gemini CLI set up and configured to interact with this vault.)

### Installation

1.  **Clone this repository:**
    ```bash
    git clone https://github.com/your-username/gemini-obsidian.git
    cd gemini-obsidian
    ```
2.  **Open the vault in Obsidian:**
    -   Open Obsidian.
    -   Click "Open another vault" -> "Open folder as vault".
    -   Navigate to and select the `gemini-obsidian` directory you just cloned.
3.  **Configure Gemini CLI:**
    -   Ensure your Gemini CLI is pointed to this directory. The `.gemini/settings.json` file is already present for basic configuration. You might need to adjust your Gemini CLI's environment or configuration to recognize and execute the custom workflows defined in `GEMINI.md`.

## Usage

Once set up, you can interact with your knowledge base using Obsidian for manual note-taking and the Gemini CLI for automated tasks.

To trigger a workflow, use the custom commands within your Gemini CLI environment:

-   `workflow:create_literature_note_from_daily`
-   `workflow:draft_permanent_note`
-   `workflow:update_index_note`

(Refer to your Gemini CLI documentation for how to execute these custom commands.)

## Contributing

Contributions are welcome! If you have ideas for new workflows, improvements, or bug fixes, please open an issue or submit a pull request.

## License

[Optional: Add your license information here, e.g., MIT License]
