---
name: transcribe
description: Transcribe handwritten or printed documents (PDFs, scans, photos) to text, or extract their tables, with HandwritingOCR. Use when the user asks to read, transcribe, digitise or OCR a document or image, convert handwriting to text, or get tables out of a scan, for one file or a folder of files.
argument-hint: '[file or folder] [tables]'
---

# Transcribe documents with HandwritingOCR

Use the `handwritingocr` MCP tools to turn documents into text or tables. If the tools are not connected, tell the user to connect the HandwritingOCR server (the plugin's MCP server) and sign in, then stop.

## 1. Find the files

- The user gives a file, several files or a folder. `$ARGUMENTS` can contain a path and the word `tables`.
- Supported types: pdf, jpg, jpeg, png, tif, tiff, bmp, gif, webp, avif. The limit is 20 MB per file.
- For a folder, list the supported files in it (not in subfolders unless the user asks). Tell the user about files that you skip and why.

## 2. Choose the action

- `transcribe` (the default): the full text of each page.
- `tables`: use this when the user asks for tables, a spreadsheet, CSV or Excel.

## 3. Ask before you use many credits

Each processed page uses one of the user's page credits. Each submission creates a new document, also when the file is the same.

- Before you submit more than 5 files, tell the user how many files you will submit and ask them to confirm.
- Never submit a file again automatically after an error. Ask the user first.

## 4. Submit each file

If you can run shell commands, use an upload URL. Do not read the file and do not encode it as base64.

1. Call `create-upload-url` with the action. Each URL works for one file only, and expires after 10 minutes.
2. Run the `curl` command it returns. Put the file path in place of `<path-to-file>`, in single quotes:
    ```bash
    curl -sS -F 'file=@/path/to/scan.pdf' '<upload_url>'
    ```
3. The response is JSON. It contains a `document_id`, or an `error` that tells you what to do. Report errors to the user in plain words.

If you cannot run shell commands, call `submit-document` with the file as base64 `content` and its `filename`. Do this only for small files (less than 1 MB). For larger files, tell the user to upload them at https://dashboard.handwritingocr.com, or to use Claude Code.

For many files, submit all of them first, then wait for the results.

## 5. Wait for the results

- Call `get-document` with the `document_id` until `status` is `processed` or `failed`.
- Wait about 10 seconds before the first check, and 15 to 30 seconds between checks. With a shell, use `sleep` between checks. Do not check in a fast loop.
- If a document is still not processed after 10 minutes, tell the user, give them the `document_id` and stop waiting.

## 6. Give the results to the user

- **Transcribe:** for a short result, show the text. For a long result or many files, save each result as a Markdown file next to the original (for example `scan.pdf` → `scan.md`) and tell the user where the files are. Ask before you overwrite a file.
- **Tables:** call `export-document` with format `xlsx` (or `json` when the user wants data), then download the file from the `download_url` with `curl -sS -o '<path>' '<download_url>'`. Save it next to the original. The download URL expires after 1 hour.
- Do not change the text that HandwritingOCR returns. If a word looks wrong, you can tell the user, but keep the original text in the file.

## Errors

- **Not enough credits:** tell the user that their HandwritingOCR account needs more page credits. They can add them at https://dashboard.handwritingocr.com.
- **Unsupported or unreadable file:** tell the user which file and why. Continue with the other files.
- **Upload URL expired or already used:** call `create-upload-url` again for that file.
