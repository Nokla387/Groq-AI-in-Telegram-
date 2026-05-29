# Groq-AI-in-Telegram-

Bot in telegram with one script

How do I create keys?

Groq key: we visit this website https://console.groq.com/keys and there we register / log in to the account, then we create a key under any name, be sure to copy and paste it into the script.

Telegram bot: log in to the bot "BotFather" creating a bot, when you have created your bot, you will be given a key, copy it and paste it into the script.

BE SURE TO DOWNLOAD THE POCKET FOR PYTHON: First this one: "pip install python-telegram-bot groq" and then this one: "pip install python-telegram-bot groq --upgrade"How do I download it? You need to install these software in the console, it will download the packages, IF YOU DO NOT DO THIS, THE SCRIPT WILL NOT WORK!

How do I insert the keys? At the beginning of the code you will see this line:
# Configuration
TELEGRAM_TOKEN = "Telegram token"
GROQ_API_KEY = "Groq key"
MODEL_ID = "llama-3.3-70b-versatile"
MAX_HISTORY = 10
You copy your keys and paste where it says in quotes "Groq key" and "Telegram key" .

A universal python script, it was created for the Groq bot, based on the model: llama-3.3-70b-versatile . the bot will work in the Telegram messenger. It will be very convenient for everyday life.
You can modify this script. 1 bug that I noticed is that he may sometimes not give an answer if asked in groups.
