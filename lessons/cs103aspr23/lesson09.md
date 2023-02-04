# Lesson 9 - Python Object Oriented Programming
This lesson continues discussion of OOP in Python but we also show how to use GPT to help learn new programming concepts, like OOP.

## the GPT API
The following Python program allows you to access the GPT server from Python.
To use it you will need to get an API key from openAI.com as mentioned in the comment

``` python
'''
Demo code for interacting with GPT-3 in Python.
This code comes from an article on medium.com
https://medium.com/codingthesmartway-com-blog/how-to-use-chatgpt-with-python-1213b8477f7b

To run this you need to 
* first visit openai.com and get an APIkey, which you insert into the code below.
  get the apikey at this URL https://platform.openai.com/account/api-keys
* next create a folder and put this file in the folder as gpt_demo.py
* finally run the following commands

% pip3 install openai  (on Mac, or pip install openai on Windows/Linux)
% python3 gpt_demo.py  (on Mac, or python gptapi.py on Windows/Linux)

The program will ask for a prompt, send it to openAI, get the response, and print it.
'''
import openai

# Set up the OpenAI API client
openai.api_key = "YOUR_API_KEY"

# test for an API key
if openai.api_key=="YOUR_API_KEY":
    print("You need to get an openapi api key to use this demo")
    exit()

# Set up the model and prompt
model_engine = "text-davinci-003"
prompt = input("Enter a prompt: ") # "Hello, how are you today?"

# Generate a response
completion = openai.Completion.create(
    engine=model_engine,
    prompt=prompt,
    max_tokens=1024,
    n=1,
    stop=None,
    temperature=0.5,
)

response = completion.choices[0].text
print(response)
```
