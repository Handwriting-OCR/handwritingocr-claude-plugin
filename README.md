# HandwritingOCR plugin for Claude

Turn scans and photos of handwritten or printed documents into text and tables, from Claude Code, Cowork or claude.ai.

## What it contains

- **MCP server:** connects Claude to your HandwritingOCR account at `https://mcp.handwritingocr.com`. You sign in with your HandwritingOCR account the first time Claude uses it. Claude acts as you, so your page credits and plan apply.
- **`transcribe` skill:** tells Claude how to submit files, wait for results and save them. Use it with `/handwritingocr:transcribe <file or folder>`, or ask in plain words, for example "transcribe the PDFs in ~/Scans" or "get the tables out of invoice.pdf".

## What you can do

- Transcribe handwritten or printed pages to text.
- Extract tables to Excel or JSON.
- Process one file or a whole folder.

Supported files: PDF, JPG, PNG, TIFF, BMP, GIF, WebP and AVIF, up to 20 MB each. Each processed page uses one page credit from your account.

In Claude Code and Cowork, Claude sends files from your disk directly to HandwritingOCR with a single-use upload link, so large files do not go through the conversation.

## Install

Install it from the Claude plugin directory, or in Claude Code:

```
/plugin marketplace add Handwriting-OCR/handwritingocr-claude-plugin
/plugin install handwritingocr@handwritingocr
```

You need a HandwritingOCR account. Create one at https://www.handwritingocr.com.

## Privacy and support

- Privacy policy: https://www.handwritingocr.com/privacy
- Terms of service: https://www.handwritingocr.com/terms
- Help: https://www.handwritingocr.com/help/basics
- Support: support@handwritingocr.com
