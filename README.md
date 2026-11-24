# Screenshot Optimizer

A local screenshot-management application for capturing, organizing, searching, and reviewing screenshots. It provides an Inbox workflow, customizable notebooks, OCR search, voice notes, and direct screenshot download support.

> **Repository:** [ritesh10176-wq/Screenshot-optimizer](https://github.com/ritesh10176-wq/Screenshot-optimizer)

## Direct Downloads

- [Download the latest source ZIP](https://github.com/ritesh10176-wq/Screenshot-optimizer/archive/refs/heads/main.zip)
- [Download `requirements.txt`](https://raw.githubusercontent.com/ritesh10176-wq/Screenshot-optimizer/main/requirements.txt)
- [Browse the latest source code](https://github.com/ritesh10176-wq/Screenshot-optimizer/tree/main)
- [View or download releases](https://github.com/ritesh10176-wq/Screenshot-optimizer/releases)

## Current Features

- Monitors configured screenshot folders and the Windows clipboard.
- Captures screenshots into an Inbox without modifying or cropping them.
- Extracts text using Tesseract OCR.
- Organizes screenshots into notebooks such as General, Research, Project Notes, and References.
- Supports creating, deleting, and organizing notebook workflows.
- Provides quick search across OCR text, notes, filenames, and notebook names.
- Supports drag-and-drop filing and moving screenshots between notebooks.
- Allows text notes and voice memos to be attached to screenshots.
- Supports pasted images and manually edited, cropped, or rotated images.
- Provides live updates to the interface through WebSockets.
- Includes a global Windows shortcut: `Ctrl + Shift + X`.

## Technology

- **Language:** Python
- **Web framework:** FastAPI
- **Server:** Uvicorn
- **File monitoring:** Watchdog
- **OCR:** Tesseract with Pytesseract
- **Image handling:** Pillow
- **Storage:** Local filesystem and JSON metadata
- **Frontend:** Static HTML, CSS, and JavaScript served by FastAPI
- **Platform support:** Primarily Windows due to clipboard monitoring, screenshot discovery, and global hotkeys

## Access the Code

### Option 1: Clone with Git

```bash
git clone https://github.com/ritesh10176-wq/Screenshot-optimizer.git
cd Screenshot-optimizer
```

### Option 2: Download the ZIP

Use the [direct source ZIP download](https://github.com/ritesh10176-wq/Screenshot-optimizer/archive/refs/heads/main.zip), then extract the archive and open a terminal in the extracted folder.

## Run Locally

### 1. Install Python

Python 3.10 or newer is required.

Verify the installation:

```bash
python --version
```

### 2. Create a Virtual Environment

Windows:

```bash
python -m venv .venv
.venv\Scripts\activate
```

Linux or macOS:

```bash
python3 -m venv .venv
source .venv/bin/activate
```

### 3. Install Python Dependencies

From the repository root:

```bash
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
```

You can also [download the requirements file directly](https://raw.githubusercontent.com/ritesh10176-wq/Screenshot-optimizer/main/requirements.txt).

### 4. Install Tesseract OCR

OCR support requires a separate Tesseract installation.

- **Windows:** Download and run the [latest Tesseract OCR installer](https://github.com/tesseract-ocr/tesseract/releases/latest), then ensure `tesseract.exe` is available.
- **Linux:**

```bash
sudo apt install tesseract-ocr
```

- **macOS:**

```bash
brew install tesseract
```

The application checks `TESSERACT_PATH`, common Windows installation directories, and the system `PATH`.

A Windows environment variable can be set explicitly:

```powershell
setx TESSERACT_PATH "C:\Program Files\Tesseract-OCR\tesseract.exe"
```

### 5. Start the Application

The source imports modules using the `app` package. Clone or extract the repository so its parent directory is the package context, then run:

```bash
cd ..
uvicorn app.server:app --host 127.0.0.1 --port 8000
```

Open [http://127.0.0.1:8000](http://127.0.0.1:8000).

Alternatively, if you place the repository directory in a location that is already importable as `app`, use:

```bash
uvicorn app.server:app --host 127.0.0.1 --port 8000
```

## Configuration

### Screenshot Watch Folder

Set a custom screenshot folder on Windows:

```powershell
setx SCREENSHOT_WATCH_DIR "C:\Users\YourName\Pictures\Screenshots"
```

The application also detects standard Windows, OneDrive, Desktop, and Pictures locations.

### Server Settings

The server defaults to:

```text
Host: 127.0.0.1
Port: 8000
```

Override them with environment variables:

```powershell
setx SERVER_HOST "127.0.0.1"
setx SERVER_PORT "8000"
```

Restart the terminal and application after using `setx`.

## Project Structure

```text
Screenshot-optimizer/
├── requirements.txt
├── __init__.py
├── config.py
├── hotkey.py
├── ocr_engine.py
├── server.py
├── storage.py
├── watcher.py
├── static/                # Web interface
├── incoming_screenshots/  # Clipboard and manual captures
└── storage/
    ├── metadata.json
    ├── notebooks.json
    ├── inbox/
    └── notebooks/
```

## Basic Workflow

1. Start the server.
2. Take a screenshot or copy an image to the clipboard.
3. The watcher stores the image in `storage/inbox`.
4. OCR text is extracted and indexed automatically.
5. Open the web interface and review the screenshot.
6. Move it into a notebook or leave it in the Inbox.
7. Add notes or a voice memo if needed.
8. Use the search box to find screenshots quickly.

## API Reference

| Method | Endpoint | Purpose |
|---|---|---|
| `GET` | `/api/notebooks` | List notebooks |
| `POST` | `/api/notebooks` | Create a notebook |
| `DELETE` | `/api/notebooks/{name}` | Delete a notebook |
| `GET` | `/api/screenshots` | List or filter screenshots |
| `POST` | `/api/move-screenshot` | Move a screenshot |
| `POST` | `/api/screenshots/{id}/notes` | Update notes |
| `POST` | `/api/voice-note` | Attach a voice note |
| `POST` | `/api/paste` | Upload a pasted image |
| `GET` | `/api/search?q=term` | Search saved screenshots |
| `DELETE` | `/api/screenshots/{id}` | Delete a screenshot |
| `GET` | `/docs` | Interactive FastAPI documentation |

## Current Limitations

- The global shortcut is Windows-only.
- Clipboard monitoring is best supported on Windows.
- OCR depends on an external Tesseract installation.
- `static/` is expected by `server.py`; a missing frontend directory will prevent the application from starting.
- The repository does not currently include a license.
