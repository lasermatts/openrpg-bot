# Enhance your RPG experience!

This is everything you need for a Discord bot written in Python that uses the [completions API](https://beta.openai.com/docs/api-reference/completions) to have conversations with the `text-davinci-003` model, and the [moderations API](https://beta.openai.com/docs/api-reference/moderations) to filter the messages. 

The real secret sauce here comes in our `.yaml` file and the prompt engineering that went into playing with tokens.

This bot uses the [OpenAI Python Library](https://github.com/openai/openai-python) and [discord.py](https://discordpy.readthedocs.io/).

Personally, I've got this sitting on an [Amazon Lightsail Virtual Private Server](https://aws.amazon.com/lightsail/faq/) partially as an experiment in "cloud management," but also because it's so plug-and-play I trust it to be easier to manage than shoving this onto a Raspberry Pi in my basement and worrying about restarting the service every time my power goes out.

# Features
- you can change the model, the hardcoded value is `text-davinci-003`
- you _could_ use this for spicing up your TTRPG games like I do, but the sky is the limit! 
- since we're leveraging Discord, you have access to all of your previous chats in the threads you create with OpenRPG-bot!

## Chat Start To Discuss a Scenario
- `/hey` starts a public thread, with a `message` argument which is the first user message passed to the bot
- The model will generate a reply for every user message in any threads started with `/hey`
- The entire thread will be passed to the model for each request, so the model will remember previous messages in the thread -- _this is important to remember, keep your interactions in mind!_
- when the context limit is reached, or a max message count is reached in the thread, OpenRPG-bot will close the thread
- you can customize the bot instructions by modifying `config.yaml`

# Setup

1. Copy `.env.example` to `.env` and start filling in the values as detailed below
1. Go to https://beta.openai.com/account/api-keys, create a new API key, and fill in `OPENAI_API_KEY`
1. Create your own Discord application at https://discord.com/developers/applications
1. Go to the Bot tab and click "Add Bot"
    - Click "Reset Token" and fill in `DISCORD_BOT_TOKEN`
    - Disable "Public Bot" unless you want your bot to be visible to everyone
    - Enable "Message Content Intent" under "Privileged Gateway Intents"
1. Go to the OAuth2 tab, copy your "Client ID", and fill in `DISCORD_CLIENT_ID`
1. Copy the ID the server you want to allow your bot to be used in by right clicking the server icon and clicking "Copy ID". Fill in `ALLOWED_SERVER_IDS`. If you want to allow multiple servers, separate the IDs by "," like `server_id_1,server_id_2`
1. Install dependencies and run the bot
    ```
    pip install -r requirements.txt
    cd into scr
    python3 main.py
    ```
    You should see an invite URL in the console. Copy and paste it into your browser to add the bot to your server.

# Optional configuration

1. If you want moderation messages, create and copy the channel id for each server that you want the moderation messages to send to in `SERVER_TO_MODERATION_CHANNEL`. This should be of the format: `server_id:channel_id,server_id_2:channel_id_2`
1. If you want to change the personality of the bot, go to `src/config.yaml` and edit the instructions
1. If you want to change the moderation settings for which messages get flagged or blocked, edit the values in `src/constants.py`. A lower value means less chance of it triggering.

---

## **FUTURE WORK** World Build
- `/world-build` starts a public thread, with a `message` argument which is the first user message passed to the bot
- The first message is also strongly pre-seeded with some boilerplate on the first message to know that it's helping you build a game world
- The entire thread will be passed to the model for each request, so the model will remember previous messages in the thread

## **FUTURE WORK** Discretizing the Bots
- We're keeping our bot keys in a `.env`, which is super nice and quick...HOWEVER I'd like to figure out a clean way to have arbitrarily many Discord bots
- What I'm after is to change the `config.yaml` of each bot (and the Discord Bot IDs of course) but still use the same OpenAI API key since that's the most "expensive" part of this endeavor

## **FUTURE WORK** DALLE-2 Integration
- What the title says, DALLE-2 is super duper cool!
- In my head how I'd architect this is something like a Discord `/slash` command and then input a unique text, or be able to copy in a particularly vivid blurb from the `davinci-2` model and it'll then link back a few really cool AI-generated art snapshots!

---

# FAQ

---

> Why isn't my bot responding to commands?

Ensure that the channels your bots have access to allow the bot to have these permissions.
- Send Messages
- Send Messages in Threads
- Create Public Threads
- Manage Messages (only for moderation to delete blocked messages)
- Manage Threads
- Read Message History
- Use Application Commands

### Quality of Life
This was the first Discord bot I ever stood up, so I really loved following this [guide I found](https://www.ionos.com/digitalguide/server/know-how/creating-discord-bot/) explaining exactly how, and more importantly, _why_ Discord bots are created

Inside of `config.yaml` there's a fair bit of [Prompt Engineering](https://github.com/mattnigh/ChatGPT3-Free-Prompt-List) but it's far from perfect. Feel free to explore!

And if you are exploring, [OpenAI's Tokenizer Tool](https://platform.openai.com/tokenizer) is great to validate the length of your prompt!

## Cron Jobs
I've got this running as a private bot inside of a Cron Job -- this project was originally forked from the [OpenAI Discord-bot](https://github.com/openai/gpt-discord-bot) project so I had to slash and make some improvements to get this behaving happily 
