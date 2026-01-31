# YouTube Download → Zip → Gofile Upload

Download YouTube videos or playlists with [yt-dlp](https://github.com/yt-dlp/yt-dlp), zip them, and upload to [Gofile.io](https://gofile.io) — all from one Jupyter notebook. Works in Colab or locally.

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/ahmed98Osama/YT-Download-Gofile-Upload/blob/main/Download_YouTube_Videos_Using_yt_dlp.ipynb)

**Open the notebook directly:** Click the **Open in Colab** button above, or use this link in your browser:  
[https://colab.research.google.com/github/ahmed98Osama/YT-Download-Gofile-Upload/blob/main/Download_YouTube_Videos_Using_yt_dlp.ipynb](https://colab.research.google.com/github/ahmed98Osama/YT-Download-Gofile-Upload/blob/main/Download_YouTube_Videos_Using_yt_dlp.ipynb)

---

## What it does

| Step | Description |
|------|-------------|
| **Download** | Fetch videos or full playlists in best available quality (video+audio merged to MP4). |
| **Zip** | Compress into one archive or one zip per playlist/folder. |
| **Upload** | Send zips to Gofile.io and get shareable download links. |

Optional **cookies** support lets you use a logged-in session (e.g. for age-restricted or region-locked content). Optional **URLs file** lets you paste URLs (one per line) into a cell and have the full workflow read them from a file.

**Gofile** ([gofile.io](https://gofile.io)) is a file-hosting service. The notebook uploads your downloaded (and zipped) files there and prints shareable links. You do **not** need to create a Gofile account; uploads work without logging in.

---

## Prerequisites

- **Google account** — to use [Google Colab](https://colab.research.google.com) (recommended; no local install).
- **Python 3.8+** and **pip** — only if you run the notebook locally (Jupyter or VS Code).

That’s it. The notebook installs `yt-dlp` in the first cell. You do **not** need a Gofile account to upload; uploads work without logging in.

---

## Step-by-step for new users (Colab + GitHub)

Follow this if you want to run everything in the browser with no setup.

1. **Open the notebook in Colab**
   - **Option A:** Click the **Open in Colab** button at the top of this README, or use the link in the description above — the notebook opens directly in Google Colab.
   - **Option B:** Go to the [repo on GitHub](https://github.com/ahmed98Osama/YT-Download-Gofile-Upload), open `Download_YouTube_Videos_Using_yt_dlp.ipynb`, then click **Open in Colab**.
   - The notebook runs in your browser; no local install needed.

2. **Install yt-dlp**
   - Run the **first code cell** (click it and press Shift+Enter, or click the play button).
   - Wait until it finishes. You only need to do this once per session.

3. **Choose your workflow**
   - **Option A — One video or one playlist, then upload each file to Gofile:**  
     Scroll to **Section 2**. Run the code cell. When prompted, paste a YouTube video or playlist URL and press Enter. Use the default save path (just press Enter) or type a folder name. After the download finishes, each file is uploaded to Gofile and you get a link per video.
   - **Option B — Multiple URLs or playlists, zip them, then upload zips to Gofile:**  
     Scroll to **Section 3 (Full workflow)**. Optionally run the **Add cookies** and **URLs file** cells (see below), then run the **Run the workflow** code cell. You can fill `CONFIG` in that cell (URLs, paths, cookies) or leave it empty and answer the prompts. At the end you get Gofile links for the zip(s).

4. **Optional: cookies (for age-restricted or members-only videos)**
   - If you see "Sign in to confirm your age" or "Video unavailable" for some videos, use the **Add cookies (optional)** cell inside Section 3. See [Add cookies (optional)](#3-add-cookies-optional) below for the full steps (export from browser, paste, run cell, set `CONFIG["cookie_file_path"]`).

5. **Optional: URLs from a file**
   - If you have many URLs, use the **URLs file (optional)** cell inside Section 3: paste one URL per line between the triple quotes, run the cell, then run the workflow. See [URLs file (optional)](#urls-file-optional) below.

6. **Get your Gofile links**
   - After the workflow runs, the last output shows **Gofile.io Download Links**. Open those URLs in your browser to download the zip(s) or files. No Gofile account is required.

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
   - **One video or playlist, then optional upload of all files:** use **Section 2** (simple download; each file is uploaded to Gofile with the video title as filename).
   - **Multiple videos/playlists → zip → upload:** use **Section 3** (full workflow). Optionally run the **Add cookies** and **URLs file** cells inside Section 3, then run the **Run the workflow** code cell (uses `CONFIG` or interactive prompts).

---

## Using the notebook

### 1. Install yt-dlp

Run the cell that runs:

```bash
%pip install yt-dlp
```

Do this once per session (and once in Colab when you open the notebook).

---

### 2. Simple download (single video or playlist) and optional Gofile upload

- **Purpose:** Download a single video or a single playlist in best quality, then optionally upload every downloaded file to Gofile via `curl`.
- **How:** Run the cell. It will ask for:
  - YouTube video or playlist URL
  - Save folder (default: `downloads`)
- **Upload:** After download, all finished files are uploaded to Gofile with the video title as the filename (one Gofile file per video).

---

### 3. Add cookies (optional)

Use this if you need a logged-in YouTube session (e.g. age-restricted or members-only videos). The notebook can use a **Netscape-format** cookie file so yt-dlp can access content that normally requires you to be signed in.

1. **Export cookies from your browser**
   - Install a browser extension that exports cookies in **Netscape** format, e.g. [Get cookies.txt LOCALLY](https://github.com/kairi003/Get-cookies.txt-Locally) (Chrome/Edge/Firefox).
   - Open YouTube in your browser and sign in.
   - Use the extension to export cookies for `youtube.com` (and optionally `google.com`) and copy the contents (or save as a `.txt` file and open it to copy).

2. **Paste cookies in the notebook**
   - In the notebook, scroll to **Section 3 (Full workflow)**.
   - Find the **Add cookies (optional)** cell. You will see something like:
     ```python
     COOKIE_CONTENT = """
     
     """
     ```
   - Paste your cookie content **between the triple quotes** (replace the empty lines). Do not delete the triple quotes.

3. **Run the cell**
   - Run the **Add cookies** cell (Shift+Enter or the play button). It writes the cookies to a file named `cookies.txt` in Colab’s environment and prints a message.

4. **Tell the full workflow to use cookies**
   - In the **Run the workflow** code cell (the big cell below), find `CONFIG` and set:
     ```python
     "cookie_file_path": "cookies.txt",
     ```
   - If you leave it as `None`, the workflow will not use cookies. The full workflow only uses the cookie file if it **exists and is non-empty**; if the file is missing or empty, it skips cookies and prints a message, so you will not break anything by leaving the path set after clearing the pasted cookies.

---

### URLs file (optional)

Use this to provide many URLs from a text block instead of typing them in `CONFIG["video_urls"]` or at the prompts. Handy for long playlists or many links.

1. **Paste URLs in the notebook**
   - In **Section 3 (Full workflow)**, find the **URLs file (optional)** cell. You will see something like:
     ```python
     URL_CONTENT = """
     
     """
     ```
   - Paste **one YouTube URL per line** between the triple quotes (video or playlist links). Empty lines and lines starting with `#` are skipped, so you can add comments.

2. **Run the cell**
   - Run the **URLs file** cell. It writes the URLs to a file named `urls.txt` and prints how many URLs were saved.

3. **Use the URLs file in the workflow**
   - **Option A:** In the **Run the workflow** cell, set `CONFIG["video_urls_file"]` to `"urls.txt"`. The workflow will load URLs from that file when you run it.
   - **Option B:** Run the **URLs file** cell first, then run the **Run the workflow** cell without changing `CONFIG`; if `urls.txt` exists, the workflow can pick it up automatically (when the notebook sets `CONFIG["video_urls_file"]` from the URLs cell). If both `video_urls_file` and `video_urls` are set, the file is used if it exists; otherwise the list in `video_urls` is used.

---

### 4. Full workflow: Download → Zip → Upload to Gofile

This section downloads all URLs you give it, zips the results, and uploads the zips to Gofile. In the notebook it is organized as:

- **Section 3 header** — Short description.
- **Add cookies (optional)** — Paste Netscape cookies and run the cell to write `cookies.txt`. The workflow uses it if `CONFIG["cookie_file_path"]` is set and the file exists and is non-empty.
- **URLs file (optional)** — Paste one URL per line and run the cell to write `urls.txt`. The workflow uses it if `CONFIG["video_urls_file"]` is set and the file exists (or run the URLs cell first; the workflow will pick up `urls.txt` automatically).
- **Run the workflow** — Run the code cell below to download, zip, and upload (uses `CONFIG` and the optional cookies/URLs file above).

#### Option A: Use `CONFIG` (no prompts)

At the top of the full-workflow cell you’ll see:

```python
USE_CONFIG = True
CONFIG = {
    "video_urls": [
        "https://www.youtube.com/watch?v=...",
        "https://www.youtube.com/playlist?list=...",
    ],
    "video_urls_file": None,   # Or path to a text file with one URL per line (e.g. "urls.txt"); skip empty and # lines
    "base_save_path": "downloads",
    "cookie_file_path": None,   # or "cookies.txt" if you use the Add cookies cell
    "compression": "individual",  # "single" or "individual"
}
```

- **`video_urls`** — List of video or playlist URLs.
- **`video_urls_file`** — Path to a text file with one URL per line (e.g. `"urls.txt"`). Empty lines and lines starting with `#` are skipped. If set and the file exists, URLs are loaded from it; otherwise `video_urls` is used.
- **`base_save_path`** — Folder where downloads and zips go (e.g. `downloads`).
- **`cookie_file_path`** — Path to your Netscape cookie file, or `None`. Used only if the file exists and is non-empty; otherwise the workflow skips cookies and prints a message.
- **`compression`** — `"single"`: one zip for everything; `"individual"`: one zip per playlist/folder.

Set `USE_CONFIG = True` and fill `CONFIG`, then run the cell. It will not ask for URLs or paths.

#### Option B: Interactive prompts

Set `USE_CONFIG = False` or leave `CONFIG["video_urls"]` and `CONFIG["video_urls_file"]` empty/unset. When you run the cell it will ask for:

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
| `video_urls_file` | Path to a text file with one URL per line (empty and `#` lines skipped) | `None` or `"urls.txt"` |
| `base_save_path` | Directory for downloads and zips | `"downloads"` |
| `cookie_file_path` | Netscape cookie file path, or `None` | `None` or `"cookies.txt"` |
| `compression` | `"single"` = one zip; `"individual"` = one zip per folder | `"individual"` |

---

## Troubleshooting

| Issue | What to do |
|-------|------------|
| **"Requested format is not available"** | The notebook uses a permissive format with fallbacks ending in `/best` and player clients `ios`, `web`, `android`. Update yt-dlp (`%pip install -U yt-dlp`) and re-run; if a video still fails, run `yt-dlp -F URL` to see available formats. |
| **"YouTube is no longer supported in this application or device"** | The notebook uses `ios`, `web`, and `android` clients (tv/mweb are excluded when they trigger this). Use the **Add cookies** cell with a logged-in YouTube session; some videos require cookies. Update yt-dlp to the latest version. |
| **Filename contains "NA" (e.g. `001_title_NA.mp4`)** | When `upload_date` is missing (e.g. some new videos), yt-dlp uses "NA". The notebook’s progress hook renames such files to remove `_NA` (e.g. `001_title.mp4`). |
| **Cookie file not found / yt-dlp fails on cookies** | Leave `cookie_file_path` as `None` or ensure the path points to an existing file. The script ignores the path if the file doesn’t exist. |
| **Playlist “succeeds” but some videos missing** | With `ignoreerrors`, failed entries are skipped. The message says “Some playlist entries may have been skipped”; check the folder and yt-dlp output for errors. |
| **Age-restricted or members-only video** | Use the **Add cookies** cell with a Netscape export from a logged-in browser, then set `CONFIG["cookie_file_path"]` to your cookie file (e.g. `"cookies.txt"`). |
| **Colab: runtime disconnected** | For long playlists, consider splitting URLs or re-running; Colab may disconnect after long idle time. |
| **HTTP 403 / 429 or "fragment not found" in full workflow** | YouTube may rate-limit when you run downloads twice in a row. Run only one of Section 2 or Section 3 per session, or wait a few minutes between runs. Section 2 (simple) does not do a separate info-extraction step, so it can be less affected. |
| **"Could not get Gofile.io server information"** | The notebook tries multiple Gofile API endpoints, then falls back to a fixed upload URL. If upload still fails, Gofile may be down or your network may block it; try again later or check [gofile.io](https://gofile.io). |

---

## Project structure

```
YT-Download-Gofile-Upload/
├── Download_YouTube_Videos_Using_yt_dlp.ipynb   # Main notebook
├── README.md                                    # This file
├── cookies.txt                                  # Optional; created by Add cookies cell
└── urls.txt                                     # Optional; created by URLs file cell
```

Downloaded files use names like `001_Title_YYYYMMDD.mp4` (number, title, upload date). The notebook uses `playlist_reverse` so the oldest video is 001 and the newest gets the last number. Zips and Gofile uploads follow the folder/playlist names.

---

## License and credits

- **yt-dlp:** [github.com/yt-dlp/yt-dlp](https://github.com/yt-dlp/yt-dlp)  
- **Gofile:** [gofile.io](https://gofile.io)  
- **Cookies export:** e.g. [Get cookies.txt LOCALLY](https://github.com/kairi003/Get-cookies.txt-Locally)

Use this notebook in line with YouTube’s Terms of Service and Gofile’s policies. Download only content you are allowed to access and share.
