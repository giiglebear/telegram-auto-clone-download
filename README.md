# Telegram Auto Clone Download

An automated Telegram monitoring and mirroring tool that monitors multiple source channels for new media, downloads files, uploads them to a destination channel, and stores them locally.

## Features

- **Multi-Channel Monitoring**: Monitor multiple Telegram channels simultaneously
- **Automatic Media Downloading**: Automatically downloads new media files from source channels
- **Auto-Upload to Destination**: Uploads downloaded files to a specified destination channel
- **Local Storage**: Saves files to a specified local directory
- **File Size Filtering**: Skips files exceeding the configured size limit
- **Progress Tracking**: Real-time download and upload progress bars
- **Resilient Connection**: Automatic reconnection on network failures
- **Error Handling**: Graceful error recovery with upload retry logic
- **Maintenance Loop**: Automatic memory optimization and temp buffer cleanup every 24 hours
- **Text Message Cloning**: Also mirrors text messages from source channels

## Requirements

- Python 3.7+
- `telethon` library

## Installation

```bash
pip install telethon
```

## Configuration

Before running the script, edit the configuration section in `main.py`:

```python
API_ID = 00000                              # Get from Telegram API (https://my.telegram.org)
API_HASH = ""                               # Get from Telegram API
SOURCE_IDS = [-10000000000, -10000000000]   # Source channel IDs (comma-separated for multiple)
DEST_ID = -10000000000                      # Destination channel ID
MAX_FILE_SIZE = 500 * 1024 * 1024           # Maximum file size in bytes (default: 500MB)
FINAL_STORAGE = r"C:\Telegram"              # Local storage directory
```

### How to Get Channel IDs

1. Open Telegram Web: https://web.telegram.org/a/
2. Navigate to the desired channel
3. Look at the URL: `https://web.telegram.org/a/#-100XXXXXXXX`
4. The channel ID is `-100XXXXXXXX` (include the leading `-100`)

### How to Get API Credentials

1. Visit https://my.telegram.org/
2. Log in with your Telegram account
3. Go to "API development tools"
4. Create a new application
5. Copy the **API ID** and **API Hash**

## Usage

```bash
python main.py
```

The script will:
1. Connect to Telegram using your credentials
2. Verify source and destination channels
3. Start monitoring source channels for new media
4. Download, upload, and store files automatically
5. Keep running until manually stopped (Ctrl+C)

## How It Works

### Step 1: Channel Verification
The script verifies that all configured source and destination channels are accessible.

### Step 2: Monitoring Engine
The script listens for new messages in source channels and:
- **If media is detected**: Downloads the file (if under size limit), uploads it to the destination, and stores it locally
- **If text is detected**: Clones the text message to the destination channel
- **On errors**: Automatically retries (especially for upload semaphore timeouts)

### Maintenance Loop
Every 24 hours, the script:
- Clears the internal entity cache
- Runs garbage collection
- Removes temporary files from the buffer directory
- Optimizes memory usage

## Error Handling

- **File Too Large**: Files exceeding `MAX_FILE_SIZE` are skipped
- **Network Errors**: Automatically reconnects after 10 seconds
- **Upload Semaphore Timeout**: Retries up to 3 times with 10-second delays
- **Unexpected Crashes**: Restarts the engine after 5 seconds

## Storage Structure

```
C:\Telegram                    # FINAL_STORAGE directory
├── file1.mp4
├── file2.pdf
├── temp_buffer/              # Temporary download location (auto-cleaned)
│   └── (temporary files)
```

## Notes

- The script requires an active Telegram account
- First run will prompt you to authenticate with your Telegram account
- Downloaded files are temporarily stored in `temp_buffer` before moving to `FINAL_STORAGE`
- The connection is resilient and will automatically reconnect on failures
- Ensure you have sufficient disk space for the configured `MAX_FILE_SIZE`

## License

[Add your license here]

## Support

For issues or questions, please open an issue on GitHub.

---

Feel free to customize this README further (add license, support section, troubleshooting tips, etc.) and save it as `README.md` in your repository root!
