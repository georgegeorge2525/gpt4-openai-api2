# GPT4 OpenAI unofficial API

## Unofficial GPT-4 API access via chat.openai.com using Selenium

Have you **applied to GPT-4 API access** but **OpenAI is too busy to reply**? Me too, that's why I created this package. It uses [selenium webdriver](https://www.selenium.dev/) to emulate user interaction on [chat.openai.com](chat.openai.com). If the account has `ChatGPT Plus`, the driver **will use GPT-4**, otherwise it will use the default GPT-3.5.

**Note:** This unofficial API library is not endorsed by OpenAI and violates their Terms of Service. Use it at your own risk; the creator assumes no liability for any consequences. Please adhere to platform's ToS and exercise caution with unofficial resources.

The core logic was taken from the [IntelligenzaArtificiale/Free-Auto-GPT](https://github.com/IntelligenzaArtificiale/Free-Auto-GPT).

## Demo

![Demo GIF](https://user-images.githubusercontent.com/18037362/236707120-e93d40bc-b73b-4f72-bc7d-d0449a082946.gif)

## Demo script

```python
from gpt4_openai import GPT4OpenAI

# Token is the __Secure-next-auth.session-token from chat.openai.com
llm = GPT4OpenAI(token=my_session_token, headless=False)
# GPT3.5 will answer 8, while GPT4 should be smart enough to answer 10
response = llm('If there are 10 books in a room and I read 2, how many books are still in the room?')
print(response)
```

As seen on the demo gif (above), GPT-4 answers correctly.

## Langchain support

`GPT4OpenAI` actually extends `LLM` class from `langchain.llms.base`. So you can easily use this library inside langchain ecosystem. Example:

```python
from gpt4_openai import GPT4OpenAI
from langchain import LLMChain
from langchain.prompts.chat import (ChatPromptTemplate, SystemMessagePromptTemplate, AIMessagePromptTemplate, HumanMessagePromptTemplate)

template="You are a helpful assistant that translates english to pirate."
system_message_prompt = SystemMessagePromptTemplate.from_template(template)
example_human = HumanMessagePromptTemplate.from_template("Hi")
example_ai = AIMessagePromptTemplate.from_template("Argh me mateys")
human_message_prompt = HumanMessagePromptTemplate.from_template("{text}")

chat_prompt = ChatPromptTemplate.from_messages([system_message_prompt, example_human, example_ai, human_message_prompt])

# Token is the __Secure-next-auth.session-token from chat.openai.com
llm = GPT4OpenAI(token=my_session_token)

chain = LLMChain(llm=llm, prompt=chat_prompt)
print(chain.run("My name is John and I like to eat pizza."))
```

Output will be:
```
AI: Ahoy, me name be John an' I be likin' ta feast on some pizza, arr!
```

## How to get the session token

1. Go to https://chat.openai.com and open the developer tools by `F12`.
2. Find the `__Secure-next-auth.session-token` cookie in `Application` > `Storage` > `Cookies` > `https://chat.openai.com`.
3. Copy the value in the `Cookie Value` field.

![image](https://user-images.githubusercontent.com/19218518/206170122-61fbe94f-4b0c-4782-a344-e26ac0d4e2a7.png)

## OpenAI's GPT4 vs other providers

Initially, I tried [poe.com ](https://poe.com/) (private API implemented at [gpt4free](https://github.com/gptforfree/gpt4free/tree/main/quora)), but noticed that input context window is smaller than one of OpenAI ChatGPT. And the same goes for Bing's GPT4.

## Installation

To install this Python package, run the following command:

```bash
pip install gpt4-openai-api
```

## Dependencies

These dependencies get downloaded directly:

- `undetected-chromedriver` (selenium browser)
- `markdownify`
- `langchain`