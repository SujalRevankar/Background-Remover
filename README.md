# ClearCut — Background Remover

ClearCut is a small web application for removing the background from an uploaded image. It returns the processed image as a downloadable PNG with the background removed.

## Features

- Select an image or drag and drop it into the upload area.
- Preview the original image before processing.
- Send the image to the Flask backend for background removal.
- Preview the processed result and download it as `no_background.png`.
- Display processing, success, and error states in the browser.

The interface advertises PNG, JPG, and WEBP image uploads. The processed output is returned as PNG.

## How It Works

```mermaid
flowchart LR
  A[Image upload] --> B[Browser preview]
  B --> C[POST /remove-bg]
  C --> D[Flask backend]
  D --> E[rembg CPU inference]
  E --> F[Pillow PNG encoding]
  F --> G[PNG returned to browser]
  G --> H[Preview and download]
```

The browser sends the selected file as multipart form data using the field name `image`. Flask opens the image with Pillow, passes it to `rembg.remove()`, and sends the result back with the `image/png` MIME type.

## Technology Stack

- **Backend:** Python, Flask
- **Background removal:** `rembg[cpu]`
- **Image handling:** Pillow
- **Production server:** Gunicorn
- **Frontend:** HTML, CSS, and JavaScript in the Flask template

## Run Locally

**Prerequisites:** Python 3.9+.

```bash
git clone https://github.com/SujalRevankar/Background-Remover.git
cd Background-Remover

python -m venv .venv
```

Activate the environment:

```bash
# Windows PowerShell
.venv\Scripts\Activate.ps1

# macOS / Linux
source .venv/bin/activate
```

Install dependencies and start Flask:

```bash
pip install -r requirements.txt
python app.py
```

Open the local address printed by Flask in your browser.

## API

| Method | Route | Purpose |
|---|---|---|
| GET | `/` | Serves the ClearCut upload interface |
| POST | `/remove-bg` | Accepts an uploaded image and returns a transparent PNG |

Example request field: `image` (multipart/form-data).

## Deployment

The repository includes `render.yaml` for a Render web service. It installs dependencies from `requirements.txt` and runs the application using:

```bash
gunicorn app:app
```

## Repository Structure

```text
.
├── app.py              Flask routes and image processing
├── templates/
│   └── index.html      Upload, preview, and download interface
├── requirements.txt    Python dependencies
├── render.yaml         Render service configuration
└── BG_Clear_Project_Report.pdf
```
