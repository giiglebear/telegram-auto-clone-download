# Telegram Auto Clone Download

Automated Telegram channel mirroring tool with multi-channel monitoring, auto-download, and local storage capabilities.

## 📋 Overview

This tool automatically monitors one or more Telegram channels for new media and messages, downloads them, re-uploads them to a destination channel, and stores them locally. It features robust error handling, automatic reconnection, and memory optimization.

## ✨ Features

- **Multi-Channel Monitoring**: Monitor multiple source channels simultaneously
- **Auto-Download & Re-upload**: Automatically downloads media from source channels and uploads to destination
- **Local Storage**: Saves all downloaded files to a local directory
- **Text Cloning**: Copies text messages without media to the destination channel
- **File Size Control**: Configurable maximum file size limit (default: 500MB)
- **Progress Tracking**: Real-time progress bars for download and upload operations
- **Error Handling**: Comprehensive error handling with automatic reconnection on network failures
- **Memory Optimization**: Automatic maintenance loop to clean temporary files and optimize memory usage (runs daily)
- **Resilient Connection**: Automatic reconnection with configurable retry delays

## 🛠️ Installation

### Requirements
- Python 3.7+
- Telethon library

### Setup

1. Clone the repository:
```bash
git clone https://github.com/giiglebear/telegram-auto-clone-download.git
cd telegram-auto-clone-download
```

2. Install dependencies:
```bash
pip install telethon
```

## ⚙️ Configuration

Edit the `main.py` file and configure the following variables:

### 1. **Telegram API Credentials**
```python
API_ID = 00000           # Get from https://my.telegram.org/apps
API_HASH = ""            # Get from https://my.telegram.org/apps
```

### 2. **Channel IDs**
```python
SOURCE_IDS = [-10000000000, -10000000000]  # Source channels (can add multiple)
DEST_ID = -10000000000                     # Destination channel
```

**How to get Channel IDs:**
1. Go to [Telegram Web](https://web.telegram.org/a/)
2. Open the desired channel
3. Look at the URL: `https://web.telegram.org/a/#-100XXXXXXXX`
4. Copy the ID: `-100XXXXXXXX`

### 3. **Storage Configuration**
```python
MAX_FILE_SIZE = 500 * 1024 * 1024  # Maximum file size in bytes (500MB default)
FINAL_STORAGE = r"C:\Telegram"      # Local directory for storage
```

## 🚀 Usage

Run the script:
```bash
python main.py
```

### Output Example
```
Connecting to Telegram...

============================================================
   STEP 1: CHANNEL VERIFICATION
============================================================
 [SOURCE] ID: -1001234567890 | NAME: My Source Channel
 [DEST]   ID: -1009876543210 | NAME: My Destination Channel

============================================================
   STEP 2: MONITORING ENGINE ACTIVE
============================================================

[14:30:45] DETECTED: New media in 'My Source Channel'
   [1/2] Downloading: [100.0%] (5242 / 5242 KB)
   [2/2] Uploading: [100.0%] (5242 / 5242 KB)
 ✅ SUCCESS: video_123.mp4 saved to D: Drive.
```

## 🔄 How It Works

1. **Connection**: Establishes connection to Telegram with automatic reconnection
2. **Verification**: Lists and verifies all configured source and destination channels
3. **Monitoring**: Listens for new messages in source channels
4. **Processing**:
   - For media files: Downloads → Uploads → Saves locally
   - For text messages: Copies directly to destination
   - Skips files exceeding the size limit
5. **Maintenance**: Daily cleanup of temporary files and memory optimization

## 🛡️ Error Handling

The tool handles various error scenarios:

| Error | Action |
|-------|--------|
| Network/Connection Error | Waits 10 seconds and reconnects |
| Semaphore Timeout (WinError 121) | Retries upload up to 3 times |
| File Too Large | Skips file and logs message |
| General Exception | Restarts engine with 5-second delay |

## 📊 Performance Features

- **Temporary Buffer**: Uses temp_buffer directory to manage downloads during processing
- **Memory Cleanup**: Automatic entity cache clearing and garbage collection every 24 hours
- **Progress Callbacks**: Real-time KB/s progress reporting for uploads and downloads

## ⚠️ Important Notes

- Ensure you have proper permissions for source and destination channels
- The destination channel should be a supergroup/broadcast channel you own or have admin access to
- Large files may take significant time to process
- The tool runs continuously; use Ctrl+C to stop
- All downloaded files are stored in the configured `FINAL_STORAGE` directory

## 🔐 Security

- Never share your API credentials or session files
- The session file (`pc_mirror_resilient`) is created locally and contains your auth token
- Keep the configuration file secure

## 🐛 Troubleshooting

**Connection keeps dropping?**
- Check internet stability
- The tool automatically reconnects; check logs for persistent errors

**Files not uploading?**
- Verify you have admin permissions in the destination channel
- Check file size against `MAX_FILE_SIZE` limit
- Ensure sufficient storage space

**Out of memory?**
- The maintenance loop runs daily; increase cleanup frequency if needed
- Reduce `MAX_FILE_SIZE` to avoid large file buffering

## 📝 License

MIT License

Copyright (c) 2026 giiglebear

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

1. The above copyright notice and this permission notice shall be included in all
   copies or substantial portions of the Software.

2. THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
   IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
   FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
   AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
   LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
   OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
   SOFTWARE.

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## 💬 Support

For issues or questions, please open a GitHub issue.
