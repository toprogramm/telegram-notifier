# Telegram Notifier GitHub Action

This GitHub Action allows you to send notifications to Telegram directly from your workflows. It supports both simple and threaded messages.

### Quick Guide

1. **Create a bot** using **BotFather** and get the token. -> [How to Create a Telegram Bot](#how-to-create-a-telegram-bot)

2. **Add the bot to your group.**  

3. **Find the Group ID and Thread ID (if needed):**  
   - Copy a message link from the group and extract the IDs.  -> [Extract IDs](#5-get-chat-id-and-thread-id)

4. **Set up a GitHub Action:**  
   - Use the example below. -> [Example workflow](#example-workflow)
   - Add these secrets to your repository: -> [Creating Secrets in Github](#creating-secrets-in-github) 
     - `TELEGRAM_BOT_TOKEN`  
     - `TELEGRAM_CHAT_ID`  
     - `TELEGRAM_THREAD_ID` (if needed).

5. **You're ready to use the bot!**  

## Features

- Send notifications to a Telegram chat or channel.
- Optionally specify a thread ID for threaded conversations.
- Supports Markdown formatting for the message content.

---

## Inputs

| Name        | Description                   | Required | Default |
| ----------- | ----------------------------- | -------- | ------- |
| `token`     | Telegram Bot Token            | Yes      | -       |
| `chat_id`   | Telegram Chat ID              | Yes      | -       |
| `thread_id` | Telegram Thread ID (optional) | No       | -       |
| `message`   | Message to send               | Yes      | -       |

---

## Usage

### Example Workflow

```yaml
name: Notify Telegram

on:
  push:
    branches:
      - '**'

jobs:
  notify:
    runs-on: ubuntu-latest

    steps:
      - name: Send Telegram Notification
        uses: toprogramm/telegram-notifier@latest
        with:
          token: ${{ secrets.TELEGRAM_BOT_TOKEN }}
          chat_id: ${{ secrets.TELEGRAM_CHAT_ID }}
          thread_id: ${{ secrets.TELEGRAM_THREAD_ID }}
          message: |
            🔔 Branch updated: `$GITHUB_REF`
            Repository: $GITHUB_REPOSITORY
            Commit: $GITHUB_SHA
```
### Result of it
![image](https://github.com/user-attachments/assets/0bb67bf0-abbf-46cf-8413-14c6da1270fc)


### Creating Secrets in GitHub
To securely store and use sensitive information like your Telegram Bot Token and Chat ID, follow these steps:

1. Go to your GitHub repository.
2. Click on the **Settings** tab.
3. Navigate to **Secrets and variables** > **Actions**.
4. Click **New repository secret**.
5. Add the following secrets:
   - **`TELEGRAM_BOT_TOKEN`**: Your Telegram Bot Token obtained from BotFather.
   - **`TELEGRAM_CHAT_ID`**: The chat ID of the recipient (user, group, or channel).
   - **`TELEGRAM_THREAD_ID`** *(optional)*: The thread ID for threaded messages.

Once added, these secrets will be securely accessible in your workflows using the `${{ secrets.<SECRET_NAME> }}` syntax.

---

### How to Create a Telegram Bot

To use this action, you need a Telegram bot. Follow these steps to create one:

1. **Open Telegram and start a chat with [BotFather](https://t.me/botfather)**:
   - Search for `@BotFather` in Telegram and start a conversation.

2. **Create a new bot**:
   - Send the `/newbot` command to BotFather.
   - Follow the instructions to name your bot and choose a username (must end with `bot`, e.g., `MyNotifierBot`).

3. **Copy the token**:
   - After the bot is created, BotFather will provide a token. This token is needed for authentication.
   - Save this token as a GitHub secret named `TELEGRAM_BOT_TOKEN`.

4. **Optional: Configure your bot**:
   - Use commands like `/setdescription`, `/setuserpic`, and `/setcommands` in BotFather to customize your bot.


#### 5. **Get Chat ID and Thread ID** 

```md
   1. Create a group and thread, then send a message in the thread.
   2. Right-click the message and select "Copy Message Link".  
   3. You'll get the link like that (e.g., `https://t.me/c/2132431338/16/348`):
      - Chat ID: Add `-100` to the start of `2132431338` → `-1002132431338`.  
      - Thread ID: Use `16`.
```
  
   **Other methods** 
   - Add the bot to the chat (user, group, or channel) where notifications will be sent.
   - Use the Telegram Bot API or tools like [IDBot](https://t.me/myidbot) to find the chat ID.

---

### Input Details

1. **`token`**
   - Obtain this token from the BotFather on Telegram by creating a new bot.
   - Store the token securely as a secret in your GitHub repository settings (`TELEGRAM_BOT_TOKEN`).

2. **`chat_id`**
   - This is the ID of the chat (user, group, or channel) where the message should be sent.
   - Use the bot API to get the `chat_id` if unsure.

3. **`thread_id`** *(optional)*
   - If you want the message to appear in a specific thread in a group, specify the `thread_id`.

4. **`message`**
   - Supports Markdown formatting (e.g., `*bold text*`, `_italic text_`, etc.).

---

## Markdown Formatting in Messages

You can use Telegram's Markdown V2 syntax for formatting messages. Below are some examples:

| Syntax      | Example             | Output        |
| ----------- | ------------------- | ------------- |
| Bold        | `*bold text*`       | **bold text** |
| Italic      | `_italic text_`     | *italic text* |
| Inline Code | `` `inline code` `` | `inline code` |
| Link        | `[link](URL)`       | [link](URL)   |

---

## Error Handling

If the action fails:

- Ensure that the `TELEGRAM_BOT_TOKEN` and `TELEGRAM_CHAT_ID` secrets are correctly configured in your GitHub repository.
- Check the workflow logs for detailed error messages.
- Verify that the `TELEGRAM_THREAD_ID` secret is valid (if used).

---

## Advanced Tips

1. **Use in Multiple Jobs**:
   Add this action to various jobs to notify about different stages in your CI/CD pipeline.

2. **Dynamic Messages**:
   Use GitHub environment variables (e.g., `$GITHUB_REF`, `$GITHUB_REPOSITORY`) to include dynamic information in your messages.

3. **Secrets Management**:
   Always store sensitive information like `token` in GitHub Secrets to ensure security.

---

## Support

For questions or issues, open an issue in the repository: [telegram-notifier/issues](https://github.com/toprogramm/telegram-notifier/issues). 

---

## License

This action is licensed under the [MIT License](LICENSE).

