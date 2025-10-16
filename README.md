Features :
Chat UI with message history saved per session in JSON under a history/ folder, including load, rename, and delete actions in the sidebar.​
One-click new chat that resets state and starts a fresh conversation with an auto title from the first user message.​
Optional file context: upload a PDF or image to extract text via PyMuPDF or Tesseract OCR, inject it once into the next question, and then continue normal chat.​
Streaming responses from an Ollama model with inline spinner feedback and minimal latency.​

Tech stack :
Streamlit for UI and session state management.​
Ollama for local LLM chat streaming.​
PyMuPDF (fitz) for PDF text extraction.​
Pillow + Tesseract via pytesseract for image OCR.​
httpx ConnectError handling for resilient model connectivity feedback.​

Requirements :
Python 3.9+ recommended.​
System Tesseract installed and available on PATH; on Windows, the script sets a common default path automatically if present.​
Ollama installed locally with the desired model pulled (default: tinyllama).​
Packages: streamlit, ollama, httpx, pillow, pytesseract, pymupdf.​

Installation :
Install system dependencies: Tesseract OCR and Ollama. Pull a model, e.g., tinyllama.​
Create a venv and install Python packages: streamlit, ollama, httpx, pillow, pytesseract, pymupdf.​
Ensure Tesseract is accessible; on Windows the script attempts "C:\Program Files\Tesseract-OCR\tesseract.exe" automatically.​

Quick start :
Place .py at the project root; a history/ folder will be created automatically on first run.​
Run Streamlit app: streamlit run code.py.​
Open the UI, start a new chat, type a message; the first user message sets the chat title automatically.​
To use one-time context, click the paperclip, upload a PDF or image, then send your question; the file’s extracted text is used once and not repeated in responses.​

How it works :
State: st.session_state stores session_name, messages, first_message flag, file, input_text, and context_used, keeping the UI consistent across interactions.​
Persistence: messages are written to history/<chat-name>.json; the sidebar lists and manages these sessions with rename/delete pop menus.​
Context: on the next send after an upload, the app extracts text using PyMuPDF for PDFs or pytesseract for images, wraps the user’s question with a “CONTEXT” block, then marks context_used so it’s only applied once.​
Streaming: the last window of messages (bounded by MAX_KEEP) is sent to ollama.chat with stream=True, and chunks are appended to render live output.​

Configuration :
MODEL: default is "tinyllama"; change to any local Ollama model that supports chat.​
MAX_KEEP: limits how many recent messages are sent to the model each turn.​
HISTORY_DIR and MAX_NAME: control where chats are saved and the maximum session name length.​
Page config: Streamlit page title and icon are set at launch.​

UI guide :
Sidebar:
New Chat: start a fresh session.​
Saved Chats: click a name to load; use the dots button for Rename/Delete; Save/Cancel appear for renames.​
Main:
Messages render as chat bubbles for user and assistant.​
If a file is attached, a thumbnail or PDF info banner shows with a Clear button.​
Composer: paperclip opens an uploader popover for jpg/jpeg/png/pdf, text field for prompts, and a send button.​

Error handling :
Gracefully surfaces Ollama connectivity issues (httpx ConnectError) and Ollama response errors with Streamlit notifications.​
File operations (save, delete, rename, load) handle exceptions and refresh caches to keep the session list accurate.​

Project structure :
code.py — the Streamlit app, including state management, OCR/PDF extraction, Ollama streaming, persistence, and UI.​
history/ — auto-created directory storing per-chat JSON transcripts.​
