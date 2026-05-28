import logging
import sys
from telegram import Update, constants
from telegram.ext import Application, CommandHandler, MessageHandler, filters, ContextTypes
from groq import Groq

# Configuration
TELEGRAM_TOKEN = "Telegram token"
GROQ_API_KEY = "Groq key"
MODEL_ID = "llama-3.3-70b-versatile"
MAX_HISTORY = 10

# Initialization
try:
    groq_client = Groq(api_key=GROQ_API_KEY)
except Exception as e:
    sys.exit(f"Initialization error: {e}")

logging.basicConfig(
    format='%(asctime)s - %(levelname)s - %(message)s',
    level=logging.INFO
)

async def get_chat_response(messages: list) -> str:
    try:
        completion = groq_client.chat.completions.create(
            model=MODEL_ID,
            messages=messages,
            temperature=0.7
        )
        return completion.choices[0].message.content
    except Exception as e:
        logging.error(f"Groq API error: {e}")
        return "Error processing request."

async def cmd_start(update: Update, context: ContextTypes.DEFAULT_TYPE):
    await update.message.reply_text("Service active.")

async def cmd_clear(update: Update, context: ContextTypes.DEFAULT_TYPE):
    context.chat_data['history'] = []
    await update.message.reply_text("History cleared.")

async def on_message(update: Update, context: ContextTypes.DEFAULT_TYPE):
    if not update.message or not update.message.text:
        return

    chat_id = update.effective_chat.id
    bot_username = context.bot.username
    
    is_private = update.message.chat.type == constants.ChatType.PRIVATE
    is_reply = (update.message.reply_to_message and 
                update.message.reply_to_message.from_user.id == context.bot.id)
    is_mentioned = f"@{bot_username}" in update.message.text

    if not (is_private or is_reply or is_mentioned):
        return

    prompt = update.message.text.replace(f"@{bot_username}", "").strip()
    if not prompt:
        return

    if 'history' not in context.chat_data:
        context.chat_data['history'] = []
    
    history = context.chat_data['history']
    history.append({"role": "user", "content": prompt})
    
    if len(history) > MAX_HISTORY:
        history = history[-MAX_HISTORY:]
        context.chat_data['history'] = history

    await context.bot.send_chat_action(chat_id=chat_id, action=constants.ChatAction.TYPING)

    system_msg = {"role": "system", "content": "Professional assistant. Concise answers."}
    messages = [system_msg] + history
    
    response = await get_chat_response(messages)
    context.chat_data['history'].append({"role": "assistant", "content": response})

    try:
        await update.message.reply_text(response, parse_mode=constants.ParseMode.MARKDOWN)
    except Exception:
        await update.message.reply_text(response)

def main():
    application = Application.builder().token(TELEGRAM_TOKEN).build()

    application.add_handler(CommandHandler("start", cmd_start))
    application.add_handler(CommandHandler("clear", cmd_clear))
    application.add_handler(MessageHandler(filters.TEXT & ~filters.COMMAND, on_message))

    print("Application status: Polling")
    application.run_polling()

if __name__ == "__main__":
    main()
