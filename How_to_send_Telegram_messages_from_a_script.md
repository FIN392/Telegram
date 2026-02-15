# How to send Telegram messages from a script

Telegram is perfect and super easy to use for sending app notifications and alerts.

You'll only need to set up a chat between your Telegram account and a bot. Then, your application will use the Telegram API to send messages to that chat.
## <a name="id"></a>First step: Find your Telegram Chat ID

1. Open a chat with '*@userinfobot*'

2. Send text '*/start*'

3. Make a note of your ID. This is going to be '*your chat ID*'.
## <a name="bot"></a>Second, create a Telegram bot

1. Open a chat with '*@BotFather*'

2. Send text '*/newbot*'

	`Alright, a new bot. How are we going to call it? Please choose a name for your bot.`

3. Type the name of your bot, for example, 'My First Bot'

	`Good. Now let's choose a username for your bot. It must end in bot. Like this, for example: TetrisBot or tetris_bot`

4. Type the same name ending in '*bot*' or '*_bot*', for example, 'FIN392_MyFirstBot'. It must be unique

	`Done! Congratulations on your new bot. You will find it at t.me/FIN392_MyFirstBot. You can now add a description, about section and profile picture for your bot, see /help for a list of commands. By the way, when you've finished creating your cool bot, ping our Bot Support if you want a better username for it. Just make sure the bot is fully operational before you do this.`
	
	`Use this token to access the HTTP API:`
	`1234567890:ABCDEF123456abcdefg-A1-ABC123ab-A1a`
	`Keep your token secure and store it safely, it can be used by anyone to control your bot.`
	
	`For a description of the Bot API, see this page: https://core.telegram.org/bots/api`

5. Take note of '*your bot token*'

6. Start a chat with @FIN392_MyFirstBot. Click '*START*'
## Finally, test it out. Send your first 'Hello World'

Just use this URL in your browser:

```
https://api.telegram.org/bot[YOUR_BOT_TOKEN]/sendMessage?chat_id=[YOUR_CHAT_ID]&text=Hello%20World
```

If it gives you a response like this, everything went smoothly:

```
{"ok":true,"result":{"message_id":13,"from":{"id":..........
```

Also, you should have received the message in Telegram:

![Telegram Hello World](assets/Telegram-Hello_World.png)

## Now let's see how to turn this into a script

***PowerShell in Windows:***

```PowerShell
# Vars
$BOT_TOKEN="[YOUR_BOT_TOKEN]"
$CHAT_ID="[YOUR_CHAT_ID]"
$MESSAGE="Hello world"

# URL
$telegramURL="https://api.telegram.org/bot$BOT_TOKEN/sendMessage"

# Call API
$Body = @{
    "chat_id" = $CHAT_ID;
    "text" = $MESSAGE;
    "parse_mode" = "HTML"
}

Invoke-WebRequest `
    -Uri $telegramURL `
    -Method Post `
    -ContentType "application/json; charset=utf-8" `
    -Body (ConvertTo-Json -Compress -InputObject $Body)
```

***Bash in Linux:***

```bash
#!/bin/bash

# Vars
BOT_TOKEN="[YOUR_BOT_TOKEN]"
CHAT_ID="[YOUR_CHAT_ID]"
MESSAGE="Hello world"

# URL
telegramURL="https://api.telegram.org/bot$BOT_TOKEN/sendMessage"

# JSON Body data
# Note: You can construct the JSON directly in a variable or use a here-string.
# Using a single variable for simplicity:
JSON_DATA=$(cat <<EOF
{
	"chat_id": "$CHAT_ID",
	"text": "$MESSAGE$",
	"parse_mode": "HTML"
}
EOF
)

# Call API using curl
curl -X POST \
	-H "Content-Type: application/json; charset=utf-8" \
	-d "$JSON_DATA" \
	"$telegramURL"
```

---
