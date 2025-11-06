# Agora Cloud Recording Console

A comprehensive web-based console for managing Agora Cloud Recording services. This tool provides a user-friendly interface to acquire, start, update, query, and stop cloud recordings with support for multiple recording modes and cloud storage providers.

## Features

- **Multiple Recording Modes**
  - **Composite Recording**: Mixes multiple audio/video streams into a single composite recording
  - **Individual Recording**: Records each user's audio/video stream separately
  - **Web Recording**: Records web pages using Agora's Web Recorder Service

- **Full Recording Lifecycle Management**
  - Acquire recording resources
  - Start recordings with customizable configurations
  - Update active recordings (subscribe/unsubscribe users, pause/resume web recordings)
  - Query recording status and file information
  - Stop recordings and retrieve file lists

- **Cloud Storage Support**
  - Amazon S3 (all regions)
  - Alibaba Cloud OSS
  - Tencent Cloud COS
  - Microsoft Azure Blob Storage
  - Google Cloud Storage
  - Huawei Cloud OBS
  - Baidu Smart Cloud BOS

- **Live Video Playback**
  - Real-time M3U8 playlist playback during recording
  - MP4 playback after recording stops
  - Automatic URL generation for cloud storage
  - HLS.js support for non-Safari browsers

- **Additional Features**
  - Snapshot configuration for periodic image capture
  - Auto-query functionality for monitoring recording status
  - Request body copying for debugging and integration
  - Credential management with localStorage persistence
  - Visual feedback with animated cloud indicators

## Prerequisites

- A modern web browser (Chrome, Firefox, Safari, Edge)
- Agora account with:
  - App ID
  - Customer ID and Customer Secret (for RESTful API authentication)
- Cloud storage account credentials (Access Key, Secret Key, Bucket) for your chosen storage provider

## Setup

1. **Clone the repository:**
   ```bash
   git clone https://github.com/AgoraIO-Community/agora-cloud-record-console.git
   cd agora-cloud-record-console
   ```

2. **Open the application:**
   - Simply open `index.html` in your web browser, or
   - Use a static file server:
     ```bash
     npx serve
     ```
     Then navigate to the provided local URL.

3. **Configure API Credentials:**
   - Click "Set API Credentials" button
   - Enter your Agora Customer ID, Customer Secret, and App ID
   - Credentials are saved in browser localStorage

## Usage Guide

### Basic Workflow

1. **Set API Credentials**
   - Click "Set API Credentials" and enter your Agora credentials
   - These are required for all API calls

2. **Select Recording Mode**
   - Choose from Composite, Individual, or Web recording mode
   - The form fields will update based on your selection

3. **Configure Recording Settings**
   - **Required Fields:**
     - Channel Name (cname)
     - UID (Recording UID)
     - Storage Access Key, Secret Key, and Bucket
   - **Optional Fields:**
     - Token (for secure channels)
     - All other fields have recommended defaults

4. **Acquire Resource**
   - Click "Acquire" to get a recording resource ID
   - The Resource ID will be displayed and stored

5. **Start Recording**
   - Configure your recording settings (audio/video profiles, layout, etc.)
   - Click "Start" to begin recording
   - The Session ID (SID) will be displayed upon success

6. **Monitor Recording**
   - Click "Query" to check recording status
   - Enable "Play Live Video" checkbox to view live M3U8 playback
   - Query automatically refreshes every 10 seconds when active

7. **Update Recording** (Optional)
   - Modify subscribe/unsubscribe UIDs for Composite/Individual mode
   - Toggle onHold status for Web recording mode
   - Click "Update" to apply changes

8. **Stop Recording**
   - Click "Stop" to end the recording
   - File list will be displayed
   - If MP4 is enabled, the final MP4 URL will be generated automatically

### Recording Modes Explained

#### Composite Recording
- Mixes multiple audio/video streams into a single file
- Supports custom video layouts (Floating, Adaptive, Vertical, Customized)
- Configurable transcoding parameters (width, height, bitrate, FPS)
- Audio profile options (48kHz mono/stereo at various bitrates)
- Subscribe/unsubscribe specific UIDs for audio and video

#### Individual Recording
- Records each user's stream separately
- Each user gets their own audio and video files
- Supports subscribe/unsubscribe UIDs
- Configurable channel type and video stream type

#### Web Recording
- Records web pages using Agora's Web Recorder Service
- Requires a web URL to record
- Configurable video dimensions and audio profile
- Supports pause/resume via onHold parameter
- Maximum recording hour limit (default: 3 hours)

### Storage Configuration

1. **Select Storage Vendor**
   - Choose your cloud storage provider from the dropdown

2. **Select Region**
   - Choose the appropriate region for your storage bucket
   - Some vendors (Azure, Google Cloud) don't require region selection

3. **Enter Credentials**
   - Access Key (or equivalent)
   - Secret Key (or equivalent)
   - Bucket name

4. **File Name Prefix** (Optional)
   - Specify a prefix for recorded files
   - Can include folder paths (e.g., `recordings/2024/`)

### Advanced Features

#### Snapshot Configuration
- Enable snapshot capture for Composite or Individual recordings
- Configure capture interval (default: 10 seconds)
- Choose file type (JPG for Composite, JPG/PNG for Individual)

#### MP4 Recording
- Check "Include MP4 in recordingFileConfig?" to generate MP4 files
- MP4 files are created alongside HLS files
- Final MP4 URL is automatically generated after stopping

#### Live Video Playback
- Enable "Play Live Video" checkbox to view recordings in real-time
- Works with M3U8 playlists during recording
- Automatically switches to MP4 after recording stops
- Supports manual URL entry (press Enter to play)

#### Request Body Copying
- Use "Copy Start Request Body" to copy the JSON request body
- Use "Copy Update Request Body" to copy update requests
- Useful for debugging and API integration

#### Query Panel
- Manually enter Resource ID and Session ID to query any recording
- Check "Use these values for Update/Stop calls?" to override global IDs
- Query results show current recording status and file list

## API Endpoints Used

The console uses the following Agora Cloud Recording RESTful API endpoints:

- `POST /v1/apps/{appid}/cloud_recording/acquire` - Acquire recording resource
- `POST /v1/apps/{appid}/cloud_recording/resourceid/{resourceId}/mode/{mode}/start` - Start recording
- `POST /v1/apps/{appid}/cloud_recording/resourceid/{resourceId}/sid/{sid}/mode/{mode}/update` - Update recording
- `GET /v1/apps/{appid}/cloud_recording/resourceid/{resourceId}/sid/{sid}/mode/{mode}/query` - Query recording status
- `POST /v1/apps/{appid}/cloud_recording/resourceid/{resourceId}/sid/{sid}/mode/{mode}/stop` - Stop recording

## Browser Compatibility

- **Chrome/Edge**: Full support including HLS.js for M3U8 playback
- **Firefox**: Full support including HLS.js for M3U8 playback
- **Safari**: Native HLS support, no HLS.js required
- **Other browsers**: Basic functionality supported, HLS playback may vary

## Dependencies

- **Tailwind CSS** (via CDN) - Styling framework
- **HLS.js** (via CDN) - HLS video playback support for non-Safari browsers

## Security Notes

- API credentials are stored in browser localStorage (not encrypted)
- Do not share your Customer Secret or storage credentials
- Use tokens for secure channel access
- Consider using environment variables or secure credential management for production use

## Troubleshooting

### Recording Not Starting
- Verify all required fields are filled (Channel Name, UID, Storage credentials)
- Check that your App ID, Customer ID, and Customer Secret are correct
- Ensure you've acquired a resource before starting

### Live Video Not Playing
- Verify storage credentials are correct
- Check that the file exists in your storage bucket
- For M3U8 files, ensure CORS is configured on your storage bucket
- Try manually entering the URL and pressing Enter

### Query Returns Empty File List
- Files may not be generated yet (wait a few seconds and query again)
- Check that recording is actually active
- Verify storage configuration is correct

### Storage URL Generation Issues
- Ensure you've selected the correct vendor and region
- Verify bucket name is correct
- Check that your storage provider's URL format matches expectations

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## License

MIT License

## GitHub

[https://github.com/AgoraIO-Community/agora-cloud-record-console](https://github.com/AgoraIO-Community/agora-cloud-record-console)

## Related Documentation

- [Agora Cloud Recording RESTful API Documentation](https://docs.agora.io/en/cloud-recording/cloud_recording_api_rest)
- [Agora Cloud Recording Overview](https://docs.agora.io/en/cloud-recording/product_overview)
