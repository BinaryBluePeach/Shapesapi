## What is the Shapes API?

Shapes API is a programmatic way to talk to your shapes.

### Shape Platforms

The Shapes API can be used to talk to your shape, whether or not they are connected on other platforms. You can have your shape in a Telegram bot, and also talk to it through your own application.

### Credit use

NOTE: Premium shapes _WILL_ use credits.
This is no different than using your premium shape on platforms like X. So plan accordingly.

### Limitations

Since Shapes API will rely on the model the shape is configured with, we cannot support all features that a regular OpenAI compatible API would support.

In particular, Shapes API will not support (for now):

- Seeing images
- System/developer role message \- (these are already part of the shape settings)
- Multiple messages and message history \- we rely on the Shape memory for this, no need to specify these. Essentially, we ignore all other messages except the last role=”user” message in the request.
- Streaming requests \- for now shapes API will be limited to non-streaming responses only
- Tool calls \- not all models support tool calls. We *may* enable this for specific models later
- Temperature, and other parameters that can be sent normally via API \- this is to prevent unwanted changes to the Shape personality as configured by the creator in the Shape settings

### Rate limits

To ensure stability of the Shapes, the Shapes API will be heavily rate limited. If the standard limits don't work for you, we can increase those on a shape-by-shape basis, to ensure a smooth experience for all users of the shape.

Send us an email at hi@shapes.inc to ask for a rate limit increase.

## How to use the Shapes API?

### Model name

The model names used for Shapes API are in the form of `shapesinc/<shape-username>`. For example, if your shape username is `archibald-brave`, the model name to use the API with the shape would be `shapesinc/archibald-brave`.

Using the external model name above does not affect the model you selected when you created the shape. Internally, your shape will still use the same AI Engine you configured for it.

### Custom Headers

We support two custom headers that can be used to pass in the user and channel id.

The shapes client is a python client that is used to send messages to the shapes api. We have two header params that we need to pass in the request so your Shape can know who is speaking to it and where the conversation is happening. Thus if you are talking to your shape in one slack chanel, and then message to it in a second slack channel, it will reply to both channels seperately, with knowlege context sandboxed in the channel level.

```
X-User-Id: user_id
```

This is the user id of the user who sent the message. This allows the shape to know who is speaking to it.

```
X-Channel-Id: channel_id
```

This is the channel id of the channel where the message was sent. This allows the shape to know where to send the response.

### API Endpoints

We support OpenAI-compatible API endpoint that can be used with the OpenAI Python or Javascript client SDKs. The API supports only non-streaming requests.

### Base URL

`https://api.shapes.inc/v1`

### Endpoints

- Chat Completions: `/chat/completions`

### Authentication

Authentication is done via API key. The API key is passed in the `Authorization` header as a Bearer token.

### Rate limiting

You can only generate 5 API Keys and each API Key 

### Examples

You can find more examples in [https://github.com/shapesinc/api](https://github.com/shapesinc/api)

All examples use the same shape username and API key. Replace `<your-API-key>` and `<shape-username>` with your actual API key and shape username.

#### Curl

```bash
curl -X POST https://api.shapes.inc/v1/chat/completions \
     -H "Authorization: Bearer \<your-API-key\>" \
     -H "Content-Type: application/json" \
     -d '{"model": "shapesinc/<shape-username>", "messages": [{ "role": "user", "content": "Hello" }]}'
```

#### Python

```python
import openai

shapes_client = OpenAI(
    api_key="<your-API-key>",
    base_url="https://api.shapes.inc/v1/",
)

response = shapes_client.chat.completions.create(
    model="shapesinc/<shape-username>",
    messages=[
        {"role": "user", "content": "Hello"}
    ]
)

print(response)
```

#### Javascript

```Javascript
const openai = require("openai");

const shapes_client = new OpenAI({
    apiKey: "<your-API-key>",
    baseURL: "https://api.shapes.inc/v1",
});

const response = await shapes_client.chat.completions.create({
    model: "shapesinc/<shape-username>",
    messages: [
        { role: "user", content: "Hello" }
    ]
});

console.log(response);
```

# Use Cases

- You can vibe code your own Shape Apps to bring Shapes to your favorite social apps 
- You can interact with a Shape on Silly Tavern

# Request for Apps

- Linking a Shape to a Reddit Account
- Allow your Shape to code review Github PRs
- Connect a Shape to a WhatsApp account
- A Shape on Threads
