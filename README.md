# 🎬 Twitch Clipper Stream Deck Plugin

**Professional clip creation and multi-platform sharing for streamers**

Create Twitch clips instantly and automatically post them to both Twitch and YouTube Live chat with a single Stream Deck button press. Features advanced OAuth token management and automatic refresh capabilities.

![Stream Deck Plugin](https://img.shields.io/badge/Stream%20Deck-Plugin-blue?style=for-the-badge&logo=elgato)
![Twitch](https://img.shields.io/badge/Twitch-9146FF?style=for-the-badge&logo=twitch&logoColor=white)
![YouTube](https://img.shields.io/badge/YouTube-FF0000?style=for-the-badge&logo=youtube&logoColor=white)

## ✨ Features

- **🎯 One-Click Operation** - Create clips and share them with a single button press
- **⚡ Instant Twitch Clipping** - Creates clips from any live Twitch stream you have editor access to
- **🔄 Dual-Platform Posting** - Automatically posts clip URLs to:
  - Twitch chat (using your identity)
  - YouTube Live chat (on target channels)
- **🛡️ Advanced OAuth Management** - Sophisticated token handling with:
  - Automatic token refresh before expiry
  - Secure credential storage
  - Error recovery and retry logic
- **🎮 Stream Deck Integration** - Native plugin with comprehensive visual feedback
- **📱 Real-time Status Updates** - Detailed progress tracking on your Stream Deck button
- **🎯 Multi-Channel Support** - Post to specific YouTube channels (perfect for editors/moderators)

## 🚀 Quick Start

### Prerequisites

- [Stream Deck software](https://www.elgato.com/en/downloads) (v4.1+)
- [Twitch CLI](https://dev.twitch.tv/docs/cli) for easy token generation
- Editor permissions on target Twitch channels
- YouTube channel access for live chat posting

### Installation

1. **Download the plugin**
   ```bash
   git clone https://github.com/BigChiefRick/clipper-streamdeck.git
   ```

2. **Install the plugin**
   - Copy `TwitchClipper.sdPlugin` folder to:
     - **Windows:** `%APPDATA%\Elgato\StreamDeck\Plugins\`
     - **Mac:** `~/Library/Application Support/com.elgato.StreamDeck/Plugins/`

3. **Restart Stream Deck software**

4. **Add the action** to your Stream Deck from the plugin library

## ⚙️ Configuration Guide

### 🟣 Twitch Setup (Required)

**Important:** This plugin is designed for editors/moderators who create clips on behalf of streamers while posting to chat using their own identity.

1. **Create a Twitch Application**
   - Go to [Twitch Developer Console](https://dev.twitch.tv/console/apps)
   - Create a new application
   - Copy your **Client ID**

2. **Generate User Access Token**
   ```bash
   # Install Twitch CLI first
   twitch token -u -s clips:edit user:write:chat
   ```

3. **Configure in Stream Deck**
   - Right-click your button → Edit
   - **Channel Name:** Enter the streamer's channel name (e.g., "ticklefitz")
   - **Your User Access Token:** Paste the complete token from Twitch CLI
   - **Client ID:** Enter your application's Client ID

**Note:** You need editor permissions on the target channel to create clips. The plugin will post to chat using your own Twitch identity.

### 🔴 YouTube Setup (Recommended)

The YouTube integration supports posting to any live YouTube channel, making it perfect for editors managing multiple streamers.

#### Step 1: Create Google OAuth Application

1. **Set up Google Cloud Project**
   - Go to [Google Cloud Console](https://console.cloud.google.com/)
   - Create a new project or select existing
   - Enable **YouTube Data API v3**

2. **Create OAuth 2.0 Credentials**
   - Navigate to APIs & Services → Credentials
   - Click "Create Credentials" → OAuth 2.0 Client ID
   - Application type: Web application
   - Add redirect URI: `http://localhost:8080`
   - Save your **Client ID** and **Client Secret**

#### Step 2: Configure OAuth in Stream Deck

1. **Enter OAuth Credentials**
   - **Client ID:** Your Google OAuth Client ID
   - **Client Secret:** Your Google OAuth Client Secret

2. **Generate Authorization URL**
   - Click **"Generate"** button
   - Copy the generated URL to your browser

3. **Complete Authorization Flow**
   - Authorize the application in your browser
   - Copy the authorization code from the redirect URL
   - Paste the code in **"Auth Code"** field
   - Click **"Exchange"** to generate tokens

4. **Configure Target Channel (Optional)**
   - **Target Channel ID:** Enter the YouTube channel ID where clips should be posted
   - Leave blank to post to your own channel's live streams
   - Example: `UCLS7oGzsUvVXvJHW2O7F6ig` (for Ticklefitz's YouTube)

#### Step 3: Automatic Token Management

The plugin automatically handles:
- **Token Refresh:** Automatically renews tokens before expiry
- **Expiry Tracking:** Monitors token validity with 5-minute buffer
- **Error Recovery:** Graceful handling of authentication failures

## 🎯 Usage

1. **Start streaming** on Twitch (and optionally YouTube)
2. **Ensure you have editor permissions** on the target Twitch channel
3. **Press your Stream Deck button** during the live stream
4. **Monitor the progress:**
   - Button shows real-time status updates
   - Clip creation and posting happens automatically
   - Success confirmation when complete

### Detailed Status Messages

| Message | Meaning |
|---------|---------|
| `Start` | Beginning clip creation process |
| `Get Auth User` | Retrieving your user authentication details |
| `Get Broadcaster` | Getting target broadcaster information |
| `Clip` | Creating the 30-second clip |
| `Wait` | Allowing clip to process on Twitch |
| `URL` | Retrieving the final clip URL |
| `TW Chat` | Posting clip URL to Twitch chat |
| `YT Check Token` | Validating/refreshing YouTube token |
| `YT Chat` | Posting clip URL to YouTube Live chat |
| `All Posted!` | Successfully posted to both platforms |
| `TW Posted!` | Posted to Twitch only (no YouTube configured) |
| `No Config` | Missing required Twitch settings |
| `No YT Live` | No active YouTube live stream found |
| `YT No Token` | YouTube token invalid or missing |

### Error Codes

| Code | Platform | Meaning |
|------|----------|---------|
| `U401/U403` | Twitch | Token expired or invalid permissions |
| `C403/C404` | Twitch | Not live streaming or clips disabled |
| `TW 401` | Twitch | Chat posting permission denied |
| `YT 401/403` | YouTube | OAuth token issues |
| `YT CH 403` | YouTube | Chat posting permission denied |

## 🔧 Troubleshooting

### Twitch Issues

**"No Config" Error**
- Verify all three Twitch fields are filled in
- Regenerate your token if it's older than 60 days

**Clip Creation Fails (C403/C404)**
- Confirm the target channel is currently live streaming
- Verify you have editor permissions on the channel
- Check that clips are enabled in the broadcaster's settings
- Ensure your token has `clips:edit` scope

**Chat Posting Fails (TW 401)**
- Verify your token has `user:write:chat` scope
- Check that you're not banned from the channel

### YouTube Issues

**"YT No Token" Error**
- Complete the OAuth flow in the settings
- Check if your token has expired and needs refresh

**"No YT Live" Error**
- Verify the target channel is currently live streaming on YouTube
- Confirm the channel ID is correct (if specified)
- Ensure the live stream has chat enabled

**YouTube Chat Posting Fails (YT 403)**
- Verify you have permission to post in the target channel's chat
- Check if the channel has restricted chat to subscribers/members

**Token Refresh Issues**
- Ensure your OAuth application is still active
- Verify the refresh token hasn't been revoked
- Re-run the OAuth flow if automatic refresh fails

### General Issues

**Plugin Not Responding**
- Restart Stream Deck software
- Check that all required fields are configured
- Review Stream Deck logs for detailed error information

**Slow Performance**
- Network connectivity issues may cause delays
- Twitch API rate limits can slow down requests
- YouTube API quotas may temporarily restrict access

## 🛠️ Technical Details

### File Structure
```
TwitchClipper.sdPlugin/
├── manifest.json          # Plugin metadata and action definitions
├── index.html            # Main plugin logic and API integrations
├── app.html             # Settings interface with OAuth flow
└── README.md           # Documentation (this file)
```

### API Integrations

**Twitch Helix API**
- Clip creation with broadcaster targeting
- Chat message posting with sender identity
- User authentication and permission validation

**YouTube Data API v3**
- Live stream detection and chat ID resolution
- OAuth 2.0 flow with automatic token refresh
- Live chat message posting with cross-channel support

**Google OAuth 2.0**
- Authorization code flow implementation
- Automatic token refresh with 5-minute expiry buffer
- Secure credential storage and management

### Required Permissions

**Twitch Scopes:**
- `clips:edit` - Create clips on channels where you have editor access
- `user:write:chat` - Post messages to chat using your identity

**YouTube Scopes:**
- `https://www.googleapis.com/auth/youtube` - Full YouTube API access for live chat

### Security Features

- OAuth tokens stored securely in Stream Deck settings
- Automatic token refresh prevents expired credential issues
- No hardcoded credentials or API keys
- Minimal required permissions for each platform

## 🤝 Contributing

Contributions are welcome! This project is ideal for:
- DevOps engineers familiar with OAuth flows
- Infrastructure developers working with streaming APIs
- Anyone interested in Stream Deck plugin development

### Development Setup

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/EnhancedFeature`)
3. Test with real Twitch/YouTube streams
4. Commit your changes (`git commit -m 'Add enhanced OAuth handling'`)
5. Push to the branch (`git push origin feature/EnhancedFeature`)
6. Open a Pull Request

### Areas for Contribution

- Additional platform integrations (Discord, Slack, etc.)
- Enhanced error handling and retry logic
- UI/UX improvements for the settings interface
- Performance optimizations for high-volume usage

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- [Elgato Stream Deck SDK](https://developer.elgato.com/documentation/stream-deck/) - Plugin framework and development tools
- [Twitch Helix API](https://dev.twitch.tv/docs/api/) - Clip creation and chat integration
- [YouTube Data API v3](https://developers.google.com/youtube/v3) - Live chat and stream management
- [Twitch CLI](https://dev.twitch.tv/docs/cli) - Simplified token generation workflow
- [Google OAuth 2.0](https://developers.google.com/identity/protocols/oauth2) - Secure authentication framework

## 📞 Support & Community

**For Technical Issues:**
- 🐛 [Report bugs](https://github.com/BigChiefRick/clipper-streamdeck/issues)
- 💡 [Request features](https://github.com/BigChiefRick/clipper-streamdeck/issues)
- 📖 [View documentation](https://github.com/BigChiefRick/clipper-streamdeck/wiki)

**For Infrastructure Teams:**
This plugin demonstrates production-ready patterns for:
- OAuth 2.0 implementation in desktop applications
- Multi-platform API integration
- Automated credential management
- Real-time status monitoring

**Community Support:**
- ⭐ Star the repository if you find it useful
- 📢 Share with fellow streamers and developers
- 🔄 Submit pull requests for improvements

---

**Built with ❤️ for the streaming and developer communities**

*Perfect for streamers, editors, and DevOps teams managing multi-platform content workflows*
