# V2T - Voice-to-Text Keyboard Extension

A highly reliable and minimal custom iOS Keyboard Extension for voice input using Groq's Whisper API. This project provides a seamless press-and-hold workflow for converting speech to text in any iOS app.

## 🎯 Features

- **Press and Hold Workflow**: Simple press-and-hold to record, release to transcribe
- **Real-time Feedback**: Visual and haptic feedback during recording and processing
- **High-Quality Transcription**: Uses Groq's whisper-large-v3 model for accurate speech-to-text
- **Secure API Key Storage**: API key stored securely using Keychain and UserDefaults
- **Minimal UI**: Clean, intuitive interface focused on the core functionality
- **Dark Mode Support**: Full support for both light and dark mode appearances
- **Error Handling**: Comprehensive error handling with user-friendly messages
- **iOS 17+ Compatibility**: Updated to use latest audio permission APIs

## 📁 Project Structure

```
V2T/
├── V2T/                          # Main Host App (SwiftUI)
│   ├── V2TApp.swift             # App entry point
│   ├── ContentView.swift        # API key configuration UI
│   └── KeychainManager.swift    # Secure API key storage
├── MyKeyboard/                   # Keyboard Extension
│   ├── KeyboardViewController.swift  # Main keyboard implementation
│   └── Info.plist               # Extension configuration
├── V2T.xcodeproj/               # Xcode project
├── README.md                    # This file
├── LICENSE                      # MIT License
└── .gitignore                   # Git ignore file
```

## 🚀 Quick Start

### Prerequisites
- Xcode 14.0 or later
- iOS 14.0 or later
- Apple Developer Account (for device testing)
- Groq API key (provided in the app)

### 1. Clone the Repository
```bash
git clone https://github.com/yourusername/V2T.git
cd V2T
```

### 2. Open in Xcode
```bash
open V2T.xcodeproj
```

### 3. Configure App Groups (Required)
1. Select the project root in Xcode navigator
2. Go to "Signing & Capabilities" for both targets:
   - V2T (main app)
   - MyKeyboard (extension)
3. Add "App Groups" capability
4. Create a new group: `group.com.voicekey.shared`
5. Enable this group for both targets

### 4. Update Bundle Identifiers
Update the bundle identifiers in project settings:
- Main App: `com.yourcompany.V2T`
- Extension: `com.yourcompany.V2T.MyKeyboard`

### 5. Build and Run
1. Build and run the main app on your device
2. The app opens with the API key pre-populated
3. Tap "Save API Key" to store it securely
4. Go to Settings > General > Keyboard > Keyboards > Add New Keyboard
5. Select "MyKeyboard" and enable "Allow Full Access"

## 📖 Usage Guide

### Host App Configuration
1. Launch the V2T app
2. The Groq API key is pre-populated: `GROQ_API`
3. Tap "Save API Key" to store it securely
4. The keyboard extension will now have access to the API key

### Using the Keyboard
1. Open any app that accepts text input (Messages, Notes, etc.)
2. Switch to the V2T keyboard by tapping the globe icon
3. **Press and hold** the microphone button to start recording
4. **Release** the button to stop recording and transcribe
5. The transcribed text will be inserted automatically

## 🔧 Technical Implementation

### Core Workflow
1. **Press Down**: 
   - Request microphone permission (iOS 17+ compatible)
   - Start AVAudioRecorder with 16kHz mono settings
   - Provide haptic feedback
   - Show recording UI state

2. **Release Up**:
   - Stop recording immediately
   - Show processing state with activity indicator
   - Send audio to Groq API

3. **API Processing**:
   - Create multipart/form-data request
   - Send to `https://api.groq.com/openai/v1/audio/transcriptions`
   - Use whisper-large-v3 model
   - Authenticate with Bearer token

4. **Completion**:
   - Insert transcribed text using `UITextDocumentProxy.insertText()`
   - Show success feedback
   - Clean up temporary audio file

### Key Components
- **KeyboardViewController**: Main keyboard implementation with UI and recording logic
- **KeychainManager**: Handles secure API key storage using Keychain
- **ContentView**: Host app interface for API key configuration
- **Audio Recording**: Uses AVAudioRecorder with optimized settings for Groq API

### API Integration
- **Endpoint**: `https://api.groq.com/openai/v1/audio/transcriptions`
- **Model**: `whisper-large-v3`
- **Format**: Multipart form data with audio file and model parameter
- **Authentication**: Bearer token in Authorization header

## 🔑 API Key Configuration

### Provided API Key
The app comes pre-configured with a Groq API key:
```
GROQ_API
```

### Using Your Own API Key
1. Get your API key from [Groq Console](https://console.groq.com/)
2. Open the V2T app
3. Replace the pre-populated key with your own
4. Tap "Save API Key"

### Security
- API keys are stored securely in the iOS Keychain
- Shared between app and extension using UserDefaults with app groups
- No sensitive data is logged or stored permanently
- All network requests use HTTPS

## 📋 Requirements

- **iOS**: 14.0 or later
- **Xcode**: 14.0 or later
- **Permissions**: Microphone access
- **Network**: Internet connection for API calls
- **API Key**: Groq API key (provided)

## 🔒 Permissions

The keyboard extension requires:
- **Microphone Access**: For voice recording
- **Open Access**: To insert text into other apps
- **Network Access**: For API calls to Groq

## 🌙 Dark Mode Support

The keyboard extension fully supports both light and dark mode appearances:

### Automatic Adaptation
- **System Appearance**: Automatically detects system-wide dark mode settings
- **App-Specific**: Adapts to individual app dark mode preferences
- **Real-time Updates**: Changes appearance immediately when settings change

### Visual Features
- **Background Colors**: Adaptive background colors for optimal contrast
- **Text Colors**: Automatic text color adaptation for readability
- **Button Styling**: Enhanced shadows and styling for each appearance
- **Activity Indicator**: Color adaptation for visibility

### Accessibility
- **High Contrast**: Ensures proper contrast ratios in both modes
- **VoiceOver Support**: Full accessibility support in both appearances
- **Dynamic Type**: Respects user's preferred text size settings

For detailed implementation information, see [DARK_MODE_SUPPORT.md](DARK_MODE_SUPPORT.md).

## 🛠️ Troubleshooting

### Common Issues

1. **Keyboard not appearing**
   - Ensure "Allow Full Access" is enabled in Settings > General > Keyboard
   - Check that the keyboard is added to your keyboard list

2. **Recording not working**
   - Check microphone permissions in Settings > Privacy & Security > Microphone
   - Ensure the app has microphone access

3. **API errors**
   - Verify the API key is saved correctly in the main app
   - Check your internet connection
   - Verify the Groq API key is valid

4. **No text insertion**
   - Ensure the keyboard has "Open Access" enabled
   - Check that you're in a text input field

5. **App crashes**
   - Ensure App Groups are properly configured
   - Check that both targets have the same App Group ID
   - Verify bundle identifiers are unique

### Debug Steps
1. Check Xcode console for error messages
2. Verify app group configuration
3. Test microphone permissions in Settings
4. Check network connectivity
5. Verify API key is properly saved

## 📝 Assumptions and Limitations

### Assumptions Made
1. **iOS Version**: Minimum iOS 14.0 support
2. **Device Capabilities**: Microphone and internet access required
3. **User Behavior**: Users will grant necessary permissions
4. **API Availability**: Groq API will remain available and stable
5. **Audio Quality**: 16kHz mono audio is sufficient for transcription
6. **Network**: Stable internet connection during transcription
7. **App Groups**: Developer will configure app groups properly

### Known Limitations
1. **Audio Length**: No explicit limit, but very long recordings may timeout
2. **Language Support**: Depends on Whisper model capabilities
3. **Background Processing**: Limited by iOS keyboard extension constraints
4. **Offline Usage**: Requires internet connection for API calls
5. **File Size**: Large audio files may cause memory issues
6. **Concurrent Usage**: Only one recording session at a time

### Technical Constraints
1. **Memory**: Keyboard extensions have limited memory
2. **CPU**: Audio processing happens on main thread
3. **Network**: API calls are synchronous (blocking)
4. **Storage**: Temporary audio files are cleaned up after use
5. **Permissions**: Requires user to grant microphone access

## 🧪 Testing

### Test Scenarios
1. **Basic Functionality**
   - Record short phrases
   - Test different languages
   - Verify text insertion

### Test Devices
- iPhone (various models)
- iPad (if supported)
- Different iOS versions (14.0+)

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Add tests if applicable
5. Submit a pull request

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- [Groq](https://groq.com/) for providing the Whisper API
- [OpenAI](https://openai.com/) for the Whisper model
- Apple for the iOS Keyboard Extension framework

## 📞 Support

For issues and questions:
1. Check the troubleshooting section above
2. Search existing GitHub issues
3. Create a new issue with detailed information

## 🔄 Version History

- **v1.0.0**: Initial release with basic voice-to-text functionality
- **v1.1.0**: Added iOS 17+ compatibility and improved error handling
- **v1.2.0**: Enhanced security with Keychain storage and better UI feedback

---

**Note**: This project is provided as-is for educational and development purposes. Please ensure you comply with Groq's API terms of service when using their services.
# Voice-to-text
<div align="center">
  <video src="(https://drive.google.com/file/d/15cCnr-fK_AL_fY3qcK6LCHdVTtLVHDj-/view?usp=sharing)" width="600" controls></video>
</div>
