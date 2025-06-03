# 🎬 Twitch Clipper Stream Deck Plugin

**One-button clip creation and multi-platform sharing for streamers**

Create Twitch clips instantly and automatically post them to multiple chat platforms with a single Stream Deck button press.

![Stream Deck Plugin](https://img.shields.io/badge/Stream%20Deck-Plugin-blue?style=for-the-badge&logo=elgato)
![Twitch](https://img.shields.io/badge/Twitch-9146FF?style=for-the-badge&logo=twitch&logoColor=white)
![YouTube](https://img.shields.io/badge/YouTube-FF0000?style=for-the-badge&logo=youtube&logoColor=white)

## ✨ Features

- **🎯 One-Click Operation** - Create clips and share them with a single button press
- **⚡ Instant Twitch Clipping** - Creates clips from your live Twitch stream
- **🔄 Multi-Platform Posting** - Automatically posts clip URLs to:
  - Twitch chat
  - YouTube Live chat
- **🎮 Stream Deck Integration** - Native plugin with visual feedback
- **🔐 Secure OAuth Authentication** - Uses official APIs with proper permissions
- **📱 Real-time Status Updates** - See exactly what's happening on your Stream Deck button

## 🚀 Quick Start

### Prerequisites

- [Stream Deck software](https://www.elgato.com/en/downloads) (v4.1+)
- [Twitch CLI](https://dev.twitch.tv/docs/cli) for easy token generation
- Active Twitch and/or YouTube channels for streaming

### Installation

1. **Download the plugin**
   ```bash
   git clone https://github.com/BigChiefRick/clipper-streamdeck.git
   ```

2. **Install the plugin**
   - Copy `clipper-streamdeck.sdPlugin` folder to:
     - **Windows:** `%APPDATA%\Elgato\StreamDeck\Plugins\`
     - **Mac:** `~/Library/Application Support/com.elgato.StreamDeck/Plugins/`

3. **Restart Stream Deck software**

4. **Add the action** to your Stream Deck from the plugin library

## ⚙️ Setup Guide

### 🟣 Twitch Configuration

1. **Create a Twitch Application**
   - Go to [Twitch Developer Console](https://dev.twitch.tv/console/apps)
   - Create a new application
   - Copy your **Client ID**

2. **Generate OAuth Token**
   ```bash
   # Install Twitch CLI first
   twitch token -u -s clips:edit user:write:chat
   ```

3. **Configure in Stream Deck**
   - Right-click your button → Edit
   - Enter your Twitch channel name
   - Paste the OAuth token (includes `oauth:` prefix)
   - Enter your Client ID

### 🔴 YouTube Configuration (Optional)

1. **Create Google OAuth Client**
   - Go to [Google Cloud Console](https://console.cloud.google.com/)
   - Enable YouTube Data API v3
   - Create OAuth 2.0 Client ID
   - Set redirect URI: `http://localhost:8080`

2. **Configure in Stream Deck**
   - Enter Client ID and Secret
   - Click "Generate" → authorize in browser
   - Paste auth code → click "Exchange"
   - (Optional) Enter target channel ID for posting to other channels

## 🎯 Usage

1. **Start streaming** on Twitch (and/or YouTube)
2. **Press your Stream Deck button** when you want to create a clip
3. **Watch the magic happen:**
   - Creates 30-second Twitch clip
   - Posts clip URL to Twitch chat
   - Posts clip URL to YouTube Live chat (if configured)
   - Shows "All Posted!" when complete

### Status Messages

| Message | Meaning |
|---------|---------|
| `Start` | Beginning clip creation |
| `Get ID` | Getting broadcaster information |
| `Clip` | Creating the clip |
| `TW Chat` | Posting to Twitch chat |
| `YT Chat` | Posting to YouTube chat |
| `All Posted!` | Success on all platforms |
| `TW Posted!` | Twitch only (no YouTube configured) |
| `No Config` | Missing required settings |

## 🔧 Troubleshooting

### Common Issues

**Button shows "No Config"**
- Check that all required Twitch fields are filled in
- Regenerate your Twitch token if it's expired

**Clip creation fails (C403/C404)**
- Make sure you're currently live streaming
- Verify your token has `clips:edit` permission
- Check that clips are enabled in your Twitch settings

**Chat posting fails (TW 401/YT 401)**
- Twitch: Token needs `user:write:chat` scope
- YouTube: Complete the OAuth flow for proper permissions

**YouTube "No YT Live" error**
- Make sure you're streaming live on YouTube
- Verify the target channel ID (if specified)

### Getting Help

1. Check the [Issues](https://github.com/BigChiefRick/clipper-streamdeck/issues) page
2. Enable Stream Deck logs for detailed error information
3. Verify your API tokens are still valid

## 🛠️ Technical Details

### File Structure
```
TwitchClipper.sdPlugin/
├── manifest.json          # Plugin metadata
├── index.html            # Main plugin logic
├── app.html             # Settings interface
└── README.md           # This file
```

### API Integrations

- **Twitch Helix API** - Clip creation and chat posting
- **YouTube Data API v3** - Live chat integration
- **OAuth 2.0** - Secure authentication for both platforms

### Permissions Required

**Twitch:**
- `clips:edit` - Create clips
- `user:write:chat` - Post to chat

**YouTube:**
- `https://www.googleapis.com/auth/youtube` - Full YouTube access

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- [Elgato Stream Deck SDK](https://developer.elgato.com/documentation/stream-deck/) - Plugin framework
- [Twitch API](https://dev.twitch.tv/docs/api/) - Clip creation and chat
- [YouTube API](https://developers.google.com/youtube/v3) - Live chat integration
- [Twitch CLI](https://dev.twitch.tv/docs/cli) - Easy token generation

## 📞 Support

If you find this plugin helpful, consider:
- ⭐ Starring the repository
- 🐛 Reporting bugs
- 💡 Suggesting new features
- 📢 Sharing with fellow streamers

---

**Made with ❤️ for the streaming community**
