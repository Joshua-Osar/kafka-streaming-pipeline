# kafka-streaming-pipeline


This repository provides a pipeline for fetching YouTube video statistics, streaming them to Kafka, and sending notifications to a Telegram channel using Telegram's API.

## Files Overview

- **`constance.py`**: Manages configuration settings, including API keys and playlist details.
- **`Youtubeanalytics.py`**: Fetches video data from YouTube and streams it to Kafka. It also sends notifications to a Telegram channel using the Telegram API.
- **`docker-compose.yml`**: Sets up the environment using Docker Compose, including services for Kafka, Zookeeper, and additional Confluent services.

## Prerequisites

- Python 3.8+
- Docker and Docker Compose installed
- YouTube Data API Key
- Telegram Bot API Token
- A Telegram chat ID where notifications will be sent

## Configuration

The configuration is managed by the `config/config.local` file, which must include both YouTube and Telegram API settings.

Example `config.local`:

```ini
[youtube]
API_KEY = your_youtube_api_key_here
PLAYLIST_ID = your_playlist_id_here

[telegram]
BOT_TOKEN = your_telegram_bot_token_here
CHAT_ID = your_telegram_chat_id_here
```

### Parameters:
- **`API_KEY`**: Your YouTube Data API key.
- **`PLAYLIST_ID`**: The YouTube playlist ID from which to fetch data.
- **`BOT_TOKEN`**: The API token of your Telegram bot.
- **`CHAT_ID`**: The chat ID of the Telegram group or channel where the data will be sent.

## Usage

1. **Set up Kafka environment**:  
   Start the Kafka, Zookeeper, and additional Confluent services using Docker Compose:
   ```bash
   docker-compose up -d
   ```

2. **Run the YouTube Analytics Script**:  
   Execute the Python script to fetch YouTube video statistics and send them to both Kafka and Telegram:
   ```bash
   python Youtubeanalytics.py
   ```

   - Video statistics will be sent to the Kafka topic `youtube_videos`.
   - Notifications with video data will be sent to the specified Telegram chat.

## Docker Services

The `docker-compose.yml` defines the following services:

- **Zookeeper**: Coordinates and manages Kafka brokers.
- **Kafka Broker**: Handles the streaming of video statistics from the YouTube API.
- **Confluent Schema Registry**: Stores and retrieves Avro schemas, used for Kafka data serialization.
- **Kafka Connect**: Manages connectors to integrate Kafka with other systems.
- **Confluent Control Center**: A web-based tool to monitor and manage Kafka clusters.

The services are configured with the necessary ports to allow local development.

### Running Kafka Services

To start the environment, use:
```bash
docker-compose up -d
```

To stop the services:
```bash
docker-compose down
```

## Telegram Integration

The script sends formatted video statistics, including views, likes, and comments, to a Telegram channel using the bot's API. The data sent to Telegram includes:
- Video title
- Number of likes, views, and comments
- Thumbnail of the video

## Future Improvements

- Add support for more video metadata and real-time dashboards.
- Provide more detailed error handling for Telegram message sending.

## License

MIT License.
