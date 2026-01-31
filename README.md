# Pipecat ↔️ Vobiz (Plivo) ↔️ STT-LLM-TTS Pipeline

A real-time voice AI pipeline that integrates Pipecat with Vobiz (Plivo) for telephony, enabling AI-powered voice conversations with Speech-to-Text, Language Model processing, and Text-to-Speech capabilities.

## Architecture

This application creates a seamless pipeline between:

- **Telephony**: Vobiz (Plivo) for phone call handling
- **Speech-to-Text**: Deepgram for voice transcription
- **Language Model**: OpenAI for intelligent responses
- **Text-to-Speech**: Cartesia for natural voice synthesis
- **Framework**: Pipecat for orchestrating the real-time media pipeline

## Prerequisites

- Python 3.8+
- ngrok (for local development)
- Active accounts and API keys for:
  - Deepgram
  - OpenAI
  - Cartesia
  - Vobiz/Plivo

## Environment Variables

Create a `.env` file in the project root with the following credentials:

```env
DEEPGRAM_API_KEY=your_deepgram_api_key
OPENAI_API_KEY=your_openai_api_key
CARTESIA_API_KEY=your_cartesia_api_key
VOBIZ_AUTH_ID=your_vobiz_auth_id
VOBIZ_AUTH_TOKEN=your_vobiz_auth_token
SERVER_URL=your_server_url
```

## Installation

1. **Clone the repository**

```bash
   git clone https://github.com/AVVKavvk/Pipecat-Vobiz-Plivo-STT-LLM-TTS-Pipeline.git

   cd Pipecat-Vobiz-Plivo-STT-LLM-TTS-Pipeline
```

2. **Create and activate virtual environment**

```bash
   python -m venv .venv
   source .venv/bin/activate  # On Windows: .venv\Scripts\activate
```

3. **Install dependencies**

```bash
   pip install -r requirements.txt
```

## Running the Application

### Local Development

1. **Start the FastAPI server**

```bash
   uvicorn main:app --host 0.0.0.0 --port 8080 --reload
```

2. **Expose your local server using ngrok**

```bash
   ngrok http 8080
```

Copy the ngrok URL (e.g., `https://abc123.ngrok.io`) - this is your `SERVER_URL`.

3. **Configure Vobiz/Plivo**
   - Log in to your Vobiz/Plivo dashboard
   - Navigate to your application settings
   - Set the Answer URL to: `{SERVER_URL}/incoming-call`
   - Save the configuration

## Usage

### Incoming Calls

Once configured, any calls to your Vobiz/Plivo number will be automatically handled by the AI pipeline.

1. Call your Vobiz/Plivo phone number
2. The call will be routed to `{SERVER_URL}/incoming-call`
3. Start speaking - the AI will transcribe, process, and respond in real-time

### Outbound Calls

Make outbound calls programmatically using the API endpoint:

**Endpoint**: `POST {SERVER_URL}/vobiz/outbound-call`

**Request Body**:

```json
{
  "from_number": "+1234567890",
  "to_number": "+0987654321",
  "body": "Optional message or context"
}
```

**Example using curl**:

```bash
curl -X POST {SERVER_URL}/vobiz/outbound-call \
  -H "Content-Type: application/json" \
  -d '{
    "from_number": "+1234567890",
    "to_number": "+0987654321",
    "body": "Making an AI-powered call"
  }'
```

## Project Structure

```
.
├── main.py                 # FastAPI application entry point
├── bot.py                  # Pipecat Config and Pipeline
├── websocket.py            # Websocket Config
├── requirements.txt        # Python dependencies
├── .env                    # Environment variables (create this)
├── .env.exmaple            # Sample Environment variables
├── .venv/                  # Virtual environment (auto-generated)
└── README.md               # This file
```

## Troubleshooting

### Common Issues

- **Connection errors**: Ensure ngrok is running and the URL is correctly configured in Vobiz/Plivo
- **Authentication failures**: Verify all API keys in `.env` file are correct and active
- **Audio issues**: Check that Deepgram and Cartesia services are operational
- **Module not found**: Ensure virtual environment is activated and dependencies are installed

### Logs

Check the console output where `uvicorn` is running for detailed logs and error messages.

## Development

To run in development mode with auto-reload:

```bash
uvicorn main:app --host 0.0.0.0 --port 8080 --reload
```

## Production Deployment

For production deployment:

1. Remove the `--reload` flag
2. Use a production WSGI server (e.g., Gunicorn)
3. Set up proper environment variable management
4. Configure HTTPS with proper SSL certificates
5. Update `SERVER_URL` to your production domain

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## Support

For issues, questions, or contributions, please open an issue in the repository.

---

**Note**: This is a development setup guide. For production deployments, additional security and scalability considerations should be implemented.
