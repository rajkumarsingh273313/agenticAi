# agenticAi

Agentic AI workshop — a lightweight Python project that exposes an AI agent through FastAPI and LangServe. The agent is restricted to Indian weather and cinema topics and uses Google Generative AI for reasoning, with tool-based access for weather lookup, movie search, and temperature conversion.

## Project overview
This project demonstrates a simple agentic workflow using LangChain and FastAPI:
- A Google Gemini/Gemma model powers the agent
- The agent can call tools to answer questions related to:
  - weather in Indian cities
  - Indian movie recommendations by genre
  - Celsius to Fahrenheit conversion
- The app exposes the agent through a REST endpoint using LangServe

## Stack
- Language: Python
- API framework: FastAPI
- ASGI server: Uvicorn
- Agent framework: LangChain + LangServe
- LLM provider: Google Generative AI (`langchain-google-genai`)
- External APIs used: Open-Meteo geocoding and weather data

## Repository structure
```bash
agenticAi/
├── app.py                # FastAPI app, tools, agent setup, and routes
├── requirements.txt      # Python dependencies
├── README.md            # Project documentation
└── .venv/               # local virtual environment (if created)
```

## Features and tools
The agent includes these tools in `app.py`:

- `get_weather(city: str) -> str`
  - Resolves a city name using Open-Meteo geocoding
  - Retrieves live weather details
  - Returns JSON with city, temperature in Celsius, and weather code

- `search_movies(genre: str) -> str`
  - Returns Indian movies by genre such as `sci-fi`, `comedy`, and `action`

- `change__to_f(temp_c: float) -> float`
  - Converts Celsius to Fahrenheit

The system prompt restricts the agent to Indian weather and cinema only:
- If the user asks any other topic, it must respond exactly with:
  `I am not authorized to answer questions outside of Indian weather and cinema.`

## How it works
The application does the following:
1. Creates a `ChatGoogleGenerativeAI` model using `GOOGLE_API_KEY`
2. Registers custom tools for weather, movies, and temperature conversion
3. Builds an agent using LangChain
4. Wraps the agent into a LangServe route at `/agent`
5. Uses FastAPI to expose the endpoint for chat-style requests

## Setup
### 1) Clone the repository
```bash
git clone https://github.com/rajkumarsingh273313/agenticAi.git
cd agenticAi
```

### 2) Create a virtual environment
```bash
python -m venv .venv
source .venv/bin/activate
# On Windows:
# .venv\Scripts\activate
```

### 3) Install dependencies
```bash
pip install -r requirements.txt
```

### 4) Set environment variables
```bash
export GOOGLE_API_KEY="your_google_api_key_here"
export PORT=8000
```

## Run the app
```bash
python app.py
```

Or:
```bash
uvicorn app:app --host 0.0.0.0 --port 8000
```

## API usage
The app exposes the agent at:
```bash
http://localhost:8000/agent
```

Example request:
```bash
curl -X POST http://localhost:8000/agent \
  -H "Content-Type: application/json" \
  -d '{"input":"What is the weather in Mumbai?"}'
```

Example response:
```json
{
  "messages": [
    {"content": "The temperature in Mumbai is 30°C and weather code is 0."}
  ]
}
```

## Dependencies
The project depends on:
```txt
fastapi
uvicorn
langserve
langchain-core
langchain-google-genai
langchain
requests
sse_starlette
```

## Notes
- Weather data is fetched from Open-Meteo, a free public weather API.
- Movie data is currently hardcoded in the `search_movies` tool and is not connected to a database.
- This is a workshop/demo project meant for learning agentic AI patterns.

## Future improvements
- Add database-backed movie data
- Add more domain-specific tools
- Add validation and better error handling
- Add unit tests for tool behavior
- Add authentication and rate limiting for production use

## License
This project does not currently include a license file. If you want, you can add one later (for example MIT).