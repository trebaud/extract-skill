---
name: ollama-cli-reference
description: Manages and interacts with local AI models using Ollama CLI and API. Use when you need to run, customize, or integrate LLM models locally for development, testing, or deployment workflows.
---

# Ollama CLI Reference

## Core Model Operations

### Running Models
```bash
# Run a model interactively
ollama run gemma3

# Run with specific prompt
ollama run gemma3 "What's in this image? /path/to/image.png"

# Enable thinking for reasoning models
ollama run deepseek-r1 --think "Where should I visit in Lisbon?"

# Disable thinking
ollama run deepseek-r1 --think=false "Summarize this article"

# Hide thinking trace while using model
ollama run deepseek-r1 --hidethinking "Is 9.9 bigger or 9.11?"
```

### Model Management
```bash
# Download a model
ollama pull gemma3

# List available models
ollama ls

# Remove a model
ollama rm gemma3

# List running models
ollama ps

# Stop a running model
ollama stop gemma3
```

### Embeddings
```bash
# Generate embeddings directly
ollama run embeddinggemma "Hello world"

# Pipe text to embed
echo "Hello world" | ollama run nomic-embed-text
```

## Custom Model Creation

### Basic Modelfile
Create a `Modelfile` with base model and customizations:

```
FROM llama3.2
PARAMETER temperature 1
PARAMETER num_ctx 4096
SYSTEM You are Mario from super mario bros, acting as an assistant.
```

### Create and Use Custom Model
```bash
# Create from Modelfile
ollama create mario-assistant -f Modelfile

# Run custom model
ollama run mario-assistant

# View existing model's Modelfile
ollama show --modelfile llama3.2
```

### Key Modelfile Instructions
- `FROM <model>`: Base model (required)
- `PARAMETER <param> <value>`: Model parameters
- `TEMPLATE <template>`: Custom prompt template
- `SYSTEM <message>`: System prompt
- `ADAPTER <path>`: LoRA adapter
- `MESSAGE <role> <content>`: Example messages
- `LICENSE <text>`: License information

### Essential Parameters
- `num_ctx`: Context window size (default: 2048)
- `temperature`: Creativity level (default: 0.8)
- `top_k`: Token diversity (default: 40)
- `top_p`: Nucleus sampling (default: 0.9)
- `stop`: Stop sequences

## API Integration

### REST API Chat
```bash
curl http://localhost:11434/api/chat -d '{
  "model": "gemma3",
  "messages": [{"role": "user", "content": "Hello there!"}],
  "stream": false
}'
```

### Python Integration
```python
from ollama import chat

response = chat(
    model='gemma3',
    messages=[{'role': 'user', 'content': 'Why is the sky blue?'}]
)
print(response['message']['content'])
```

### JavaScript Integration
```javascript
import ollama from 'ollama'

const response = await ollama.chat({
    model: 'gemma3',
    messages: [{role: 'user', content: 'Why is the sky blue?'}]
})
console.log(response.message.content)
```

## Advanced Capabilities

### Vision Models
```bash
# CLI with image
ollama run gemma3 ./image.png "What's in this image?"

# API with base64 image
curl -X POST http://localhost:11434/api/chat \
  -H "Content-Type: application/json" \
  -d '{
    "model": "gemma3",
    "messages": [{
      "role": "user", 
      "content": "What is in this image?",
      "images": ["base64-encoded-image"]
    }]
  }'
```

### Structured Outputs
```python
from ollama import chat
from pydantic import BaseModel

class Country(BaseModel):
    name: str
    capital: str
    languages: list[str]

response = chat(
    model='gpt-oss',
    messages=[{'role': 'user', 'content': 'Tell me about Canada.'}],
    format=Country.model_json_schema()
)
country = Country.model_validate_json(response.message.content)
```

### Tool Calling
```python
from ollama import chat

def get_temperature(city: str) -> str:
    """Get current temperature for a city"""
    temperatures = {"New York": "22°C", "London": "15°C"}
    return temperatures.get(city, "Unknown")

response = chat(
    model="qwen3", 
    messages=[{"role": "user", "content": "What's the temperature in New York?"}],
    tools=[get_temperature],
    think=True
)

if response.message.tool_calls:
    call = response.message.tool_calls[0]
    result = get_temperature(**call.function.arguments)
    # Add tool result and continue conversation
```

### Streaming
```python
stream = chat(
    model='qwen3',
    messages=[{'role': 'user', 'content': 'What is 17 × 23?'}],
    stream=True
)

for chunk in stream:
    print(chunk.message.content, end='', flush=True)
```

## Development Integration

### Quick Coding Setup
```bash
# Pull coding model
ollama pull glm-4.7-flash

# Launch with coding tool
ollama launch claude --model glm-47-flash
```

### Supported Integrations
- OpenCode (open-source coding assistant)
- Claude Code (Anthropic's agentic coding tool)  
- Codex (OpenAI's coding assistant)
- Droid (Factory's AI coding agent)

### Server Management
```bash
# Start Ollama server
ollama serve

# View environment variables
ollama serve --help

# Authentication
ollama signin
ollama signout
```

## Recommended Models

### Coding
- `glm-4.7-flash`: 23GB VRAM, 64K context
- `glm-4.7:cloud`: Full cloud model

### Embeddings
- `embeddinggemma`
- `qwen3-embedding`
- `all-minilm`

### Reasoning (with thinking)
- `qwen3`
- `deepseek-r1`
- `deepseek-v3.1`

### Multimodal
- `gemma3` (vision)
- `llama4:scout` (109B parameters)
- `qwen2.5vl`

## Quick Reference Patterns

### Interactive Session Commands
- `/set think`: Enable thinking mode
- `/set nothink`: Disable thinking mode
- `"""multiline"""`: Multiline input

### Efficient Workflows
1. **Development**: Use coding models + `ollama launch`
2. **RAG**: Use embedding models + vector search
3. **API Integration**: Use REST API or Python/JS SDKs
4. **Customization**: Create Modelfiles for specific use cases
5. **Reasoning**: Use thinking-capable models for complex problems