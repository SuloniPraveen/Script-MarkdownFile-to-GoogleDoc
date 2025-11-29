# Markdown to Google Docs Converter

A Python solution for the Software Engineer Assessment Task that converts markdown meeting notes into a well-formatted Google Doc with proper styling, checkboxes, and assignee mentions.

## Overview

This script parses markdown-formatted meeting notes and creates a professionally formatted Google Doc using the Google Docs API. It preserves the document structure including headings, nested bullet points, checkboxes, and special formatting for assignee mentions and footer information.

## Features

- **Heading Styles**: Converts `#`, `##`, and `###` to Heading 1, 2, and 3 respectively
- **Nested Bullets**: Maintains proper indentation hierarchy for multi-level bullet points
- **Checkboxes**: Converts markdown `- [ ]` into actual Google Docs checkboxes
- **Assignee Styling**: Highlights `@mentions` with bold blue text
- **Footer Formatting**: Styles footer content (after `---`) with italics and smaller font size
- **Error Handling**: Includes proper error handling for API calls

## Project Structure

```
├── Markdown_To_GoogleDoc.ipynb    # Main Colab notebook
└── README.md
└── Product Team Sync - May 15, 2023 (Auto-Generated).pdf   # Google Doc Pdf Output File
```

## Dependencies

```python
google-api-python-client
google-auth-httplib2
google-auth-oauthlib
```

These will be installed automatically when running the notebook in Google Colab.

## Setup Instructions

### Running in Google Colab

1. **Open the Notebook**

   - Upload `markdown_to_gdocs.ipynb` to Google Colab
   - Or create a new notebook and copy the code sections

2. **Authentication**

   - Run the authentication cell
   - Follow the prompts to sign in with your Google account
   - Grant the required permissions for Google Docs and Drive access

3. **Run the Script**
   - Execute all cells in order
   - The script will create a new Google Doc and provide a link to view it

### Local Development (Optional)

If running outside Colab, you'll need to set up OAuth2 credentials:

1. Create a project in [Google Cloud Console](https://console.cloud.google.com)
2. Enable the Google Docs API
3. Create OAuth 2.0 credentials
4. Download credentials and save as `credentials.json`
5. Modify authentication code to use local credentials instead of Colab auth

## How to Use

### Basic Usage

1. **Prepare Your Markdown**

   - The script includes a sample markdown document
   - Replace the `markdown_notes` variable with your own content

2. **Run the Script**

   ```python
   # Parse markdown
   blocks = parse_markdown(markdown_notes)

   # Create Google Doc
   doc_id = create_document("Your Document Title")

   # Write formatted content
   write_blocks_to_document(doc_id, blocks)
   ```

3. **Access Your Document**

   - The script outputs a direct link to your new Google Doc
   - The document is automatically saved to your Google Drive

4. **Access the Output of this Challenge**
   - https://docs.google.com/document/d/1bRRF0Y918WH1fLae1pM96mJQB964a0Kfl85f6a_Qrts/edit?tab=t.0#heading=h.rz0blxghwq2v

### Supported Markdown Features

- **Headings**: `#`, `##`, `###`
- **Bullets**: `-` or `*` with indentation support
- **Checkboxes**: `- [ ]` or `* [ ]`
- **Assignees**: `@username` format
- **Footer**: Content after `---` separator

## Code Structure

### Main Components

1. **`parse_markdown(md: str)`**

   - Parses markdown text into structured blocks
   - Detects headings, bullets, checkboxes, and footer content
   - Returns a list of dictionaries representing document structure

2. **`create_document(title: str)`**

   - Creates a new Google Doc with the specified title
   - Returns the document ID for further operations

3. **`build_requests_from_blocks(blocks: List[Dict], base_index: int)`**

   - Converts parsed blocks into Google Docs API requests
   - Handles text insertion, styling, indentation, and bullet formatting
   - Returns a list of API request objects

4. **`write_blocks_to_document(doc_id: str, blocks: List[Dict])`**

   - Executes batch update on the Google Doc
   - Applies all formatting in a single API call for efficiency

5. **`find_assignee_ranges(text: str)`**
   - Identifies `@mention` patterns in text
   - Returns character ranges for styling

## Configuration

### Styling Constants

```python
INDENT_PT = 18  # Indentation per list level (in points)

ASSIGNEE_COLOR = {
    "default": {
        "color": {
            "rgbColor": {"red": 0.0, "green": 0.2, "blue": 0.7}
        }
    }
}
```

Modify these constants to adjust indentation spacing or assignee mention colors.

## Error Handling

The script includes error handling for:

- API authentication failures
- Document creation errors
- Batch update operation failures

Errors are caught and reported with descriptive messages.

## Example Output

The script converts this markdown:

```markdown
# Product Team Sync

## Agenda

### 1. Sprint Review

- Completed Features
  - User authentication flow

* [ ] @sarah: Finalize Q3 roadmap
```

Into a formatted Google Doc with:

- "Product Team Sync" as Heading 1
- "Agenda" as Heading 2
- "1. Sprint Review" as Heading 3
- Properly indented bullets
- Checkbox for the action item
- Bold blue "@sarah" mention
