# YouTube Download → Zip → Gofile Upload

Download YouTube videos or playlists with [yt-dlp](https://github.com/yt-dlp/yt-dlp), zip them, and upload to [Gofile.io](https://gofile.io) — all from one Jupyter notebook. Works in Colab or locally.

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/ahmed98Osama/YT-Download-Gofile-Upload/blob/main/Download_YouTube_Videos_Using_yt_dlp.ipynb)

---

## What it does

| Step | Description |
|------|-------------|
| **Download** | Fetch videos or full playlists in best available quality (video+audio merged to MP4). |
| **Zip** | Compress into one archive or one zip per playlist/folder. |
| **Upload** | Send zips to Gofile.io and get shareable download links. |

Optional **cookies** support lets you use a logged-in session (e.g. for age-restricted or region-locked content).

---

## Prerequisites

- **Python 3.8+** (or use [Google Colab](https://colab.research.google.com) — no install needed).
- **pip** (for installing `yt-dlp`).

That’s it. The notebook installs `yt-dlp` in the first cell.

---

## Quick start

1. **Open the notebook**  
   - Locally: open `Download_YouTube_Videos_Using_yt_dlp.ipynb` in Jupyter or VS Code.  
   - Or click **Open in Colab** above.

2. **Install yt-dlp**  
   Run the first code cell:
   ```bash
   %pip install yt-dlp
   ```

3. **Choose how you want to run**
   - **One video, then optional upload:** use **Section 2** (simple download + commented `curl` for Gofile).
   - **Multiple videos/playlists → zip → upload:** use **Section 3** (full workflow) and set `CONFIG` or use the interactive prompts.

---

## Using the notebook

### 1. Install yt-dlp

Run the cell that runs:

```bash
%pip install yt-dlp
```

Do this once per session (and once in Colab when you open the notebook).

---

### 2. Simple single-video download (and optional Gofile upload)

- **Purpose:** Download one video in best quality, then optionally upload one file to Gofile via `curl`.
- **How:** Run the cell. It will ask for:
  - YouTube video URL
  - Save folder (default: `downloads`)
- **Upload:** At the bottom of the same cell there is a commented `curl` command. Uncomment it and set the file path to your downloaded video (e.g. `downloads/Your Video Title.mp4`) to upload that file to Gofile.

---

### 3. Add cookies (optional)

Use this if you need a logged-in YouTube session (e.g. age-restricted or members-only videos).

1. Export cookies from your browser in **Netscape** format, e.g. with [Get cookies.txt LOCALLY](https://github.com/kairi003/Get-cookies.txt-Locally).
2. In the **Add cookies** cell, paste the cookie content between the triple quotes in `COOKIE_CONTENT`.
3. Run the cell. It writes `cookies.txt` and tells you to set `CONFIG["cookie_file_path"]` in the full workflow.
4. In **Section 3**, set `CONFIG["cookie_file_path"]` to `"cookies.txt"` (or leave `None` if you don’t use cookies).  
   The full workflow only uses the cookie file if that file actually exists, so you won’t break anything by leaving the path set after clearing the pasted cookies.

---

### 4. Full workflow: Download → Zip → Upload to Gofile

This section downloads all URLs you give it, zips the results, and uploads the zips to Gofile.

#### Option A: Use `CONFIG` (no prompts)

At the top of the full-workflow cell you’ll see:

```python
USE_CONFIG = True
CONFIG = {
    "video_urls": [
        "https://www.youtube.com/watch?v=...",
        "https://www.youtube.com/playlist?list=...",
    ],
    "base_save_path": "downloads",
    "cookie_file_path": None,   # or "cookies.txt" if you use the Add cookies cell
    "compression": "individual",  # "single" or "individual"
}
```

- **`video_urls`** — List of video or playlist URLs.
- **`base_save_path`** — Folder where downloads and zips go (e.g. `downloads`).
- **`cookie_file_path`** — Path to your Netscape cookie file, or `None`. Only used if the file exists.
- **`compression`** — `"single"`: one zip for everything; `"individual"`: one zip per playlist/folder.

Set `USE_CONFIG = True` and fill `CONFIG`, then run the cell. It will not ask for URLs or paths.

#### Option B: Interactive prompts

Set `USE_CONFIG = False` or leave `CONFIG["video_urls"]` empty. When you run the cell it will ask for:

- URLs (one per line; empty line to finish)
- Base save path (default: `downloads`)
- Cookie file path (optional; Enter to skip)
- Compression: `single` or `individual`

#### What the full workflow does

1. **Download** — For each URL, gets metadata, creates a subfolder (playlist or video name), and downloads with yt-dlp (best video+audio, merged to MP4). Uses `ignoreerrors` so one bad video doesn’t stop a whole playlist.
2. **Zip** — Builds zip(s) under `base_save_path` (one zip or one per subfolder, depending on `compression`).
3. **Upload** — Uploads each zip to Gofile.io and prints the download links.

---

## Configuration reference

| Key | Description | Example |
|-----|-------------|--------|
| `video_urls` | List of YouTube video or playlist URLs | `["https://www.youtube.com/playlist?list=..."]` |
| `base_save_path` | Directory for downloads and zips | `"downloads"` |
| `cookie_file_path` | Netscape cookie file path, or `None` | `None` or `"cookies.txt"` |
| `compression` | `"single"` = one zip; `"individual"` = one zip per folder | `"individual"` |

---

## Troubleshooting

| Issue | What to do |
|-------|------------|
| **"Requested format is not available"** | The notebook uses a format selector with fallbacks (`bv*+ba*/bv+ba/b`) so when YouTube serves only combined streams (e.g. SABR), yt-dlp uses the best single file. If a video still fails, try it alone with `--list-formats` in yt-dlp to see available formats. |
| **Cookie file not found / yt-dlp fails on cookies** | Leave `cookie_file_path` as `None` or ensure the path points to an existing file. The script ignores the path if the file doesn’t exist. |
| **Playlist “succeeds” but some videos missing** | With `ignoreerrors`, failed entries are skipped. The message says “Some playlist entries may have been skipped”; check the folder and yt-dlp output for errors. |
| **Age-restricted or members-only video** | Use the **Add cookies** cell with a Netscape export from a logged-in browser, then set `CONFIG["cookie_file_path"]` to your cookie file (e.g. `"cookies.txt"`). |
| **Colab: runtime disconnected** | For long playlists, consider splitting URLs or re-running; Colab may disconnect after long idle time. |

---

## Project structure

```
YT-Download-Gofile-Upload/
├── Download_YouTube_Videos_Using_yt_dlp.ipynb   # Main notebook
└── README.md                                    # This file
```

---

## License and credits

- **yt-dlp:** [github.com/yt-dlp/yt-dlp](https://github.com/yt-dlp/yt-dlp)  
- **Gofile:** [gofile.io](https://gofile.io)  
- **Cookies export:** e.g. [Get cookies.txt LOCALLY](https://github.com/kairi003/Get-cookies.txt-Locally)

Use this notebook in line with YouTube’s Terms of Service and Gofile’s policies. Download only content you are allowed to access and share.
