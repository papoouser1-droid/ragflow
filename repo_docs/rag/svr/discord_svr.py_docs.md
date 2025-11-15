# File Documentation: rag/svr/discord_svr.py

## File Metadata

- **Path**: `rag/svr/discord_svr.py`
- **Extension**: `.py`
- **Lines**: 82
- **Characters**: 2,634
- **Size**: 2,634 bytes
- **Purpose**: Python Module - Contains classes, functions, or business logic

## Original Source

```python
#
#  Copyright 2024 The InfiniFlow Authors. All Rights Reserved.
#
#  Licensed under the Apache License, Version 2.0 (the "License");
#  you may not use this file except in compliance with the License.
#  You may obtain a copy of the License at
#
#      http://www.apache.org/licenses/LICENSE-2.0
#
#  Unless required by applicable law or agreed to in writing, software
#  distributed under the License is distributed on an "AS IS" BASIS,
#  WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
#  See the License for the specific language governing permissions and
#  limitations under the License.
#
import logging
import discord
import requests
import base64
import asyncio

URL = '{YOUR_IP_ADDRESS:PORT}/v1/api/completion_aibotk' # Default: https://demo.ragflow.io/v1/api/completion_aibotk

JSON_DATA = {
    "conversation_id": "xxxxxxxxxxxxxxxxxxxxxxxxxxx", # Get conversation id from /api/new_conversation
    "Authorization": "ragflow-xxxxxxxxxxxxxxxxxxxxxxxxxxxxx", # RAGFlow Assistant Chat Bot API Key
    "word": "" # User question, don't need to initialize
}

DISCORD_BOT_KEY = "xxxxxxxxxxxxxxxxxxxxxxxxxx" #Get DISCORD_BOT_KEY from Discord Application


intents = discord.Intents.default()
intents.message_content = True
client = discord.Client(intents=intents)


@client.event
async def on_ready():
    logging.info(f'We have logged in as {client.user}')


@client.event
async def on_message(message):
    if message.author == client.user:
        return

    if client.user.mentioned_in(message):

        if len(message.content.split('> ')) == 1:
            await message.channel.send("Hi~ How can I help you? ")
        else:
            JSON_DATA['word']=message.content.split('> ')[1]
            response = requests.post(URL, json=JSON_DATA)
            response_data = response.json().get('data', [])
            image_bool = False

            for i in response_data:
                if i['type'] == 1:
                    res = i['content']
                if i['type'] == 3:
                    image_bool = True
                    image_data = base64.b64decode(i['url'])
                    with open('tmp_image.png','wb') as file:
                        file.write(image_data)
                    image= discord.File('tmp_image.png')

            await message.channel.send(f"{message.author.mention}{res}")

            if image_bool:
                await message.channel.send(file=image)


loop = asyncio.get_event_loop()

try:
    loop.run_until_complete(client.start(DISCORD_BOT_KEY))
except KeyboardInterrupt:
    loop.run_until_complete(client.close())
finally:
    loop.close()

```

## High-Level Overview

#
#  Copyright 2024 The InfiniFlow Authors. All Rights Reserved.
#
#  Licensed under the Apache License, Version 2.0 (the "License");
#  you may not use this file except in compliance with the License.
#  You may obtain a copy of the License at
#
#      http://www.apache.org/licenses/LICENSE-2.0
#
#  Unless required by applicable law or agreed to in writing, software
#  distributed under the License is distributed on an "AS IS" BASIS,
#  WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
#  See the License for the specific language governing permissions and
#  limitations under the License.
#

## Detailed Walkthrough


### Imports (5)

- `import logging`
- `import discord`
- `import requests`
- `import base64`
- `import asyncio`

## Code Structure Analysis

- Total lines: 82
- Blank lines: 18 (22.0%)
- Comment lines: ~15 (18.3%)
- Code lines: ~49


## Dependencies and Imports

- `import logging`
- `import discord`
- `import requests`
- `import base64`
- `import asyncio`

## Design & Architecture

This file is located in the `rag` directory, specifically within `rag/svr`.

This file contributes to the overall functionality of the RAGFlow system.

## Performance & Complexity

- Contains 2 loop(s) - consider algorithmic complexity
- Uses asynchronous patterns for better performance

## Security & Safety Considerations

- **User Input**: Validate and sanitize all user input
- **Authentication**: Ensure secure password handling and authentication
- **File Operations**: Validate file paths to prevent directory traversal

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `rag/svr/` directory
- Potential test file: `test_discord_svr.py`

## Keywords

ANY, API, All, Apache, Application, Assistant, Authorization, Authors, BASIS, Bot, CONDITIONS, Chat, Client, Copyright, DISCORD_BOT_KEY, Default, Discord, False, File, Get, How, InfiniFlow, Intents, JSON_DATA, KIND, Key, KeyboardInterrupt, LICENSE, License, Licensed, PORT, Python, RAGFlow, Reserved, Rights, See, The, True, URL, Unless, User, Version, WARRANTIES, WITHOUT, YOUR_IP_ADDRESS, You, client, on_message, on_ready

---
*Generated by RAGFlow Repository Documentation Generator*
