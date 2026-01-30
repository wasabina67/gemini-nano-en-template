# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a template project for building web applications that use Chrome's **Gemini Nano on-device AI** via the Prompt API. The application is a simple chat interface that runs AI inference entirely in the browser using Chrome's built-in Gemini Nano model.

## Architecture

### Server Architecture
- **server.js**: Minimal Express.js server that serves static files from the `docs/` directory
- Runs on port 3000 by default
- No backend API - all AI inference happens client-side in the browser

### Frontend Architecture
The frontend is located in the `docs/` directory:

- **index.html**: Simple chat UI with status indicator, message container, and input form
- **app.js**: Core application logic that interfaces with Chrome's Prompt API
  - Uses the browser's native `LanguageModel` API (Chrome-specific)
  - Handles session creation with language configuration (English by default)
  - Implements streaming responses for real-time AI output
  - Uses DOMPurify and Marked.js for safe markdown rendering
- **styles.css**: Chat interface styling

### Key Configuration
The application is configured for English language in `docs/app.js`:
```javascript
const LANGUAGE = 'en';
const LANGUAGE_OPTIONS = {
  expectedInputs: [{ type: 'text', languages: [LANGUAGE] }],
  expectedOutputs: [{ type: 'text', languages: [LANGUAGE] }],
};
```

To change the language, modify the `LANGUAGE` constant and update the system prompt accordingly.

## Development Commands

### Install dependencies
```bash
npm i
```

### Run the development server
```bash
node server.js
```
Access at http://localhost:3000

## Chrome Setup Requirements

This application requires Chrome with specific flags enabled:

1. Enable Gemini Nano on-device model:
   ```
   chrome://flags/#optimization-guide-on-device-model
   ```
   Set to: **Enabled**

2. Enable Prompt API for Gemini Nano multimodal input:
   ```
   chrome://flags/#prompt-api-for-gemini-nano-multimodal-input
   ```
   Set to: **Enabled**

3. Restart Chrome after enabling these flags

4. The model will download automatically on first use

## Important Notes

- This only works in Chrome with the required flags enabled
- The `LanguageModel` API is a Chrome-specific experimental API (not a web standard)
- All AI inference happens on-device in the browser - no data is sent to external servers
- The model download is triggered automatically when first initializing the session
- Session creation includes download progress monitoring via the `monitor` callback
