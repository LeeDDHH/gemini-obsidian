# Gemini Obsidian Knowledge Management Vault

This is a personal knowledge management system built on Obsidian, following Zettelkasten principles with AI-automated workflows for processing URLs, generating summaries, and maintaining a networked knowledge base.

## Architecture

### Directory Structure

- **`Daily/`** - Inbox for daily notes containing URLs to process
- **`FleetingNote/`** - Temporary notes for capturing immediate ideas
- **`LiteratureNote/`** - AI-generated summaries from external sources (web pages, YouTube videos)
- **`PermanentNote/`** - Refined, atomic notes representing core concepts
- **`IndexNote/`** - Maps and indexes for navigating permanent notes
- **`.claude/commands/`** - Custom workflow definitions for Claude Code
- **`.cache/youtube_subs/`** - Temporary storage for YouTube transcript downloads
- **`.workflow_cache/`** - Workflow processing history for idempotency

### Workflow Pipeline

```
Daily/*.md (URLs) 
  → /workflow:create_literature_note_from_daily 
    → LiteratureNote/*.md

FleetingNote/ + LiteratureNote/ 
  → /workflow:draft_permanent_note 
    → PermanentNote/*.md

PermanentNote/*.md 
  → /workflow:update_index_note 
    → IndexNote/*.md (updated)
```

## Custom Workflows

### /workflow:create_literature_note_from_daily

**Purpose**: Extract URLs from Daily notes, fetch content, and generate Korean-language summaries in LiteratureNote/.

**Key behaviors**:
- Distinguishes YouTube (`youtube.com/watch`, `youtu.be`, `youtube.com/shorts`) from web sources
- **YouTube processing**: Uses `yt-dlp` to extract transcripts (never HTML fetch)
  - Command: `yt-dlp --skip-download --write-subs --write-auto-subs --sub-langs "ko,ja,en" --sub-format "vtt"`
  - Converts `.vtt` to plain text (removes timestamps, tags, duplicates)
  - Language priority: ko → ja → en
  - If transcript unavailable: creates note with failure reason, no summary
- **Web processing**: Fetches HTML and extracts main content
- **Summary length**: Dynamic based on content length
  - Default (< 8K chars): 500-1000 chars
  - Long (8K-20K chars): 1000-1800 chars
  - Very long (> 20K chars): Structured format with sections
- **URL cleanup**: Removes standalone URL lines from Daily notes (preserves inline URLs for context)
- **Idempotency**: Tracks processed URLs in `.workflow_cache/processed_urls.json`
- **Completion**: Plays `afplay /System/Library/Sounds/Ping.aiff` when done

### /workflow:draft_permanent_note

**Purpose**: Analyze Fleeting and Literature notes to identify themes, then draft a Permanent Note.

**Key behaviors**:
- Cross-analyzes all notes in FleetingNote/ and LiteratureNote/
- AI generates 3-7 theme clusters with explanations and evidence
- Prompts user to select theme(s)
- Creates PermanentNote with structure:
  - TL;DR
  - Current conclusion
  - Evidence/references (with source links)
  - Counter-examples/constraints
  - Practical application checklist
  - Follow-up questions

### /workflow:update_index_note

**Purpose**: Add newly created Permanent Notes to relevant Index Notes for discoverability.

**Key behaviors**:
- Reads Index Notes from IndexNote/
- Identifies new Permanent Notes (by created_at/updated_at)
- Matches notes to appropriate indexes by tags/topics/keywords
- Inserts links under appropriate headings
- Prevents duplicate links
- Maintains existing sort order

## Note Format Conventions

### Frontmatter (all note types)

```yaml
type: literature|permanent|index
source_url: <original URL>
source_title: <title>
created_at: <ISO timestamp>
updated_at: <ISO timestamp>
tags: [tag1, tag2]
topics: [topic1, topic2]
```

### Frontmatter (LiteratureNote specifics)

```yaml
source_type: youtube|web
has_transcript: true|false        # YouTube only
transcript_lang: ko|ja|en|N/A     # YouTube only
failure_reason: N/A|TRANSCRIPT_NOT_AVAILABLE|TRANSCRIPT_FETCH_FAILED|VIDEO_UNAVAILABLE|REGION_OR_AGE_RESTRICTED
```

### File naming

- **LiteratureNote**: `YYYY-MM-DD__{domain}__{slug}.md`
- **PermanentNote**: `{theme_slug}.md` (human-readable, descriptive)

### Links

- Use Obsidian wiki-style links: `[[Note Title]]`
- Preserve original URLs alongside titles when creating references

## Key Tools & Dependencies

- **yt-dlp**: Required for YouTube transcript extraction. Check installation: `yt-dlp --version`
- **Obsidian**: Desktop application for vault management
- **Language**: Content is primarily Korean (한국어), with some Japanese (日本語) notes

## Workflow Execution Rules

### Idempotency

- All workflows check `.workflow_cache/processed_urls.json` to avoid duplicate processing
- Re-running a workflow on same input skips already-processed items
- LiteratureNote frontmatter `source_url` serves as secondary check

### Error Handling

- Partial results are always saved
- Failed items are logged with failure reason in note frontmatter
- YouTube transcript failures create notes with `failure_reason` and suggested next actions

### Content Language

- All summaries and analysis are written in **Korean**
- Original source titles and URLs are preserved as-is
- Workflow descriptions in commands use Korean

## Troubleshooting

### YouTube Transcript Extraction Fails

1. Verify yt-dlp installation: `yt-dlp --version`
2. Update yt-dlp: `pip install -U yt-dlp`
3. For age-restricted videos: `yt-dlp --cookies-from-browser chrome <URL>`
4. Check `.cache/youtube_subs/` for generated `.vtt` files by video ID

### URL Not Being Processed

- Check if URL is in `.workflow_cache/processed_urls.json` (already processed)
- Verify URL is on standalone line or in markdown link format
- Check Daily note for URL format (`https://` or `http://`)

## Migration Note

This project is transitioning from Gemini CLI to Claude Code. The active workflow implementations are in `.claude/commands/`. Legacy specifications exist in `GEMINI.md` for reference.
