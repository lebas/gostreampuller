# 🎬 gostreampuller

**gostreampuller** is a lightweight Go package to download videos or extract audio from popular streaming platforms including **YouTube, Facebook, Instagram, TikTok, Twitter, and many more** using [`yt-dlp`](https://github.com/yt-dlp/yt-dlp) and [`ffmpeg`](https://ffmpeg.org/).  
It's designed for cross-platform use: **Linux, macOS, and Windows**.

---

## 📦 Installation

```bash
go get github.com/debargha2001/gostreampuller
```

---

## 🚀 Usage

### Basic Usage

```go
package main

import (
  "fmt"
  "log"

  "github.com/debargha2001/gostreampuller/downloader"
)

func main() {
  // Optional: Set full path for binaries (especially useful on Windows)
  // downloader.SetYTDLPPath("C:\\Users\\yourname\\yt-dlp.exe")
  // downloader.SetFFMPEGPath("C:\\ffmpeg\\bin\\ffmpeg.exe")

  // Basic video download with defaults (mp4, 720p, avc1 codec)
  video, err := downloader.DownloadVideo("https://youtube.com/watch?v=dQw4w9WgXcQ", "", "", "")
  if err != nil {
    log.Fatal(err)
  }
  fmt.Println("Downloaded video:", video)

  // Basic audio download with defaults (mp3, libmp3lame codec, 128k bitrate)
  audio, err := downloader.DownloadAudio("https://instagram.com/p/example", "", "", "")
  if err != nil {
    log.Fatal(err)
  }
  fmt.Println("Downloaded audio:", audio)
}
```

### Advanced Usage with Custom Parameters

```go
package main

import (
  "fmt"
  "log"

  "github.com/debargha2001/gostreampuller/downloader"
)

func main() {
  // Download 1080p video in webm format with VP9 codec
  video, err := downloader.DownloadVideo(
    "https://facebook.com/watch?v=example",
    "webm",  // format
    "1080",  // resolution
    "vp9",   // codec
  )
  if err != nil {
    log.Fatal(err)
  }
  fmt.Println("Downloaded HD video:", video)

  // Download high-quality audio in FLAC format
  audio, err := downloader.DownloadAudio(
    "https://tiktok.com/@user/video/example",
    "flac",      // output format
    "flac",      // codec
    "320k",      // bitrate
  )
  if err != nil {
    log.Fatal(err)
  }
  fmt.Println("Downloaded FLAC audio:", audio)
}
```

---

## 📋 Function Parameters

### DownloadVideo Parameters

```go
func DownloadVideo(url string, format string, resolution string, codec string) (string, error)
```

| Parameter | Description | Default | Possible Values |
|-----------|-------------|---------|-----------------|
| `url` | Video URL from supported platforms | *required* | YouTube, Facebook, Instagram, TikTok, Twitter, etc. |
| `format` | Output video format | `mp4` | `mp4`, `webm`, `mkv`, `avi`, `mov`, `flv` |
| `resolution` | Video resolution height | `720` | `144`, `240`, `360`, `480`, `720`, `1080`, `1440`, `2160` (4K) |
| `codec` | Video codec | `avc1` | `avc1` (H.264), `vp9`, `vp8`, `av01` (AV1), `hevc` (H.265) |

**Example combinations:**
```go
// HD MP4 with H.264
downloader.DownloadVideo(url, "mp4", "1080", "avc1")

// 4K WebM with VP9
downloader.DownloadVideo(url, "webm", "2160", "vp9")

// Standard definition with AV1
downloader.DownloadVideo(url, "mp4", "480", "av01")
```

### DownloadAudio Parameters

```go
func DownloadAudio(url string, outputFormat string, codec string, bitrate string) (string, error)
```

| Parameter | Description | Default | Possible Values |
|-----------|-------------|---------|-----------------|
| `url` | Video URL from supported platforms | *required* | YouTube, Facebook, Instagram, TikTok, Twitter, etc. |
| `outputFormat` | Output audio format | `mp3` | `mp3`, `aac`, `ogg`, `wav`, `flac`, `m4a` |
| `codec` | Audio codec | `libmp3lame` | `libmp3lame`, `aac`, `libvorbis`, `pcm_s16le`, `flac`, `libfdk_aac` |
| `bitrate` | Audio bitrate | `128k` | `64k`, `96k`, `128k`, `192k`, `256k`, `320k` |

**Example combinations:**
```go
// High-quality MP3
downloader.DownloadAudio(url, "mp3", "libmp3lame", "320k")

// Lossless FLAC
downloader.DownloadAudio(url, "flac", "flac", "320k")

// Efficient AAC
downloader.DownloadAudio(url, "m4a", "aac", "192k")

// Uncompressed WAV
downloader.DownloadAudio(url, "wav", "pcm_s16le", "320k")
```

### Parameter Combinations Guide

**For highest quality video:**
```go
downloader.DownloadVideo(url, "webm", "2160", "vp9")  // 4K WebM with VP9
```

**For best compatibility:**
```go
downloader.DownloadVideo(url, "mp4", "1080", "avc1")  // 1080p MP4 with H.264
```

**For smallest file size:**
```go
downloader.DownloadVideo(url, "webm", "720", "vp9")   // 720p WebM with VP9
```

**For highest quality audio:**
```go
downloader.DownloadAudio(url, "flac", "flac", "320k") // Lossless FLAC
```

**For best compatibility audio:**
```go
downloader.DownloadAudio(url, "mp3", "libmp3lame", "320k") // High-quality MP3
```

**For smallest audio file:**
```go
downloader.DownloadAudio(url, "ogg", "libvorbis", "128k") // Efficient OGG Vorbis
```

---

## 🛠️ Platform Setup

If you get this error:

```
exec: "yt-dlp": executable file not found in %PATH%
```

It means your system doesn't know where to find `yt-dlp` or `ffmpeg`.

### 🐧 Linux Setup

**Option 1: Using package managers**

**Ubuntu/Debian:**
```bash
# Install yt-dlp
sudo apt update
sudo apt install yt-dlp

# Install ffmpeg
sudo apt install ffmpeg
```

**Arch Linux:**
```bash
# Install yt-dlp
sudo pacman -S yt-dlp

# Install ffmpeg
sudo pacman -S ffmpeg
```

**Option 2: Manual installation**
```bash
# Download yt-dlp
sudo curl -L https://github.com/yt-dlp/yt-dlp/releases/latest/download/yt-dlp -o /usr/local/bin/yt-dlp
sudo chmod a+rx /usr/local/bin/yt-dlp

# Install ffmpeg (if not available via package manager)
# Download from https://ffmpeg.org/download.html
```

### 🍎 macOS Setup

**Option 1: Using Homebrew (recommended)**
```bash
# Install Homebrew if you haven't already
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# Install yt-dlp
brew install yt-dlp

# Install ffmpeg
brew install ffmpeg
```

**Option 2: Using MacPorts**
```bash
# Install yt-dlp
sudo port install yt-dlp

# Install ffmpeg
sudo port install ffmpeg
```

**Option 3: Manual installation**
```bash
# Download yt-dlp
sudo curl -L https://github.com/yt-dlp/yt-dlp/releases/latest/download/yt-dlp -o /usr/local/bin/yt-dlp
sudo chmod a+rx /usr/local/bin/yt-dlp

# For ffmpeg, download from https://ffmpeg.org/download.html
```

### 🪟 Windows Setup

**Option 1: Using package managers**

**Using Chocolatey:**
```powershell
# Install Chocolatey if you haven't already
Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))

# Install yt-dlp
choco install yt-dlp

# Install ffmpeg
choco install ffmpeg
```

**Using Scoop:**
```powershell
# Install Scoop if you haven't already
Set-ExecutionPolicy RemoteSigned -Scope CurrentUser
irm get.scoop.sh | iex

# Install yt-dlp
scoop install yt-dlp

# Install ffmpeg
scoop install ffmpeg
```

**Option 2: Manual installation**

1. **Download binaries**:
   - [yt-dlp.exe](https://github.com/yt-dlp/yt-dlp/releases/latest)
   - [ffmpeg Windows build](https://ffmpeg.org/download.html)

2. **Add to PATH or override in Go**:

```go
// If not in PATH, override binary paths in Go
downloader.SetYTDLPPath("C:\\Users\\yourname\\Desktop\\yt-dlp.exe")
downloader.SetFFMPEGPath("C:\\ffmpeg\\bin\\ffmpeg.exe")
```

### 🔧 Custom Binary Paths

If you prefer to use custom binary locations on any platform, you can override the default paths:

```go
// Linux/macOS examples
downloader.SetYTDLPPath("/usr/local/bin/yt-dlp")
downloader.SetFFMPEGPath("/usr/local/bin/ffmpeg")

// Windows example
downloader.SetYTDLPPath("C:\\Users\\yourname\\Desktop\\yt-dlp.exe")
downloader.SetFFMPEGPath("C:\\ffmpeg\\bin\\ffmpeg.exe")
```

That's it! ✅

---

## 🐳 Docker Support

For containerized environments, you can use this Dockerfile to create a container with gostreampuller, yt-dlp, and ffmpeg pre-installed:

```dockerfile
# ---------- Build Stage ----------
# Use official Golang image to compile the Go app
FROM golang:1.24.1-bullseye AS builder

# Set the working directory inside the builder container
WORKDIR /app

# Copy Go module files and download dependencies
COPY go.mod go.sum ./
RUN go mod download

# Copy source code into the builder container
COPY . .

# Build the Go binary; replace 'your-app-name' with your actual binary name
RUN go build -o your-app-name main.go

# ---------- Runtime Stage ----------
# Use a lightweight Debian image for production
FROM debian:bullseye-slim

# Install required tools:
# - python3: needed by yt-dlp
# - ffmpeg: for audio processing
# - curl: to download yt-dlp binary
# - ca-certificates: ensures HTTPS requests work
RUN apt-get update && \
    apt-get install -y python3 ffmpeg curl ca-certificates && \
    curl -L https://github.com/yt-dlp/yt-dlp/releases/latest/download/yt-dlp -o /usr/local/bin/yt-dlp && \
    chmod a+rx /usr/local/bin/yt-dlp && \
    apt-get clean && rm -rf /var/lib/apt/lists/*

# Set the working directory inside the runtime container
WORKDIR /app

# Copy the compiled Go binary from the builder stage
COPY --from=builder /app/your-app-name .

# Expose the API port (adjust port as needed)
EXPOSE 8080

# Run the Go server when the container starts
CMD ["./your-app-name"]
```

> **📝 Note**: Replace `your-app-name` with the actual name of your Go application binary. The Dockerfile above is a template that can be customized for any Go application using gostreampuller.

### 🔨 Building and Running with Docker

```bash
# Build the Docker image
docker build -t gostreampuller-app .

# Run the container
docker run -p 8080:8080 gostreampuller-app
```

**Benefits of using Docker:**
- ✅ No need to manually install yt-dlp and ffmpeg
- ✅ Consistent environment across different systems
- ✅ Easy deployment and scaling
- ✅ Isolated dependencies

---

## 🧠 Features

- 📹 Download videos in multiple formats (`.mp4`, `.webm`, `.mkv`, etc.)
- 🎧 Extract audio in various formats (`.mp3`, `.flac`, `.aac`, etc.)
- 🌐 Supports multiple platforms: **YouTube, Facebook, Instagram, TikTok, Twitter, and more**
- 🧩 Works across Windows, macOS, Linux
- 🔀 Binary path overrides for full control
- 🧼 Automatically cleans up temporary files
- ⚡ Smart format detection and conversion

---

## 🗂️ Folder Structure

```
gostreampuller/
├── downloader/
│   └── downloader.go        // Core package
├── main.go                  // Example usage
├── go.mod
└── README.md
```

---

## 📣 License

MIT License © [Debargha Dutta](https://github.com/debarghadutta)
