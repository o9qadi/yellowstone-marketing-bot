import os
import random
from telegram import Update
from telegram.ext import ApplicationBuilder, CommandHandler, ContextTypes
from dotenv import load_dotenv

load_dotenv()
BOT_TOKEN = os.getenv("BOT_TOKEN")

def generate_response(keyword):
    styles = [
        f"الوهم النفسي: اربط {keyword} بشعور يخلي العميل يحس إنه محتاجه أكثر من أي وقت.",
        f"فكرة تسويقية: اربط {keyword} بقصة شخصية أو لحظة مؤثرة.",
        f"خطة ذكية: قدم {keyword} كتجربة حصرية أو شي ما يتكرر."
    ]
    return random.choice(styles)

def get_image_url(keyword):
    return f"https://source.unsplash.com/600x400/?{keyword.replace(' ', '%20')}"

async def handle_sayd(update: Update, context: ContextTypes.DEFAULT_TYPE):
    if context.args:
        keyword = ' '.join(context.args)
        response = generate_response(keyword)
        image_url = get_image_url(keyword)
        await update.message.reply_photo(photo=image_url, caption=response)
    else:
        await update.message.reply_text("اكتب الكلمة بعد /صيد، مثل: /صيد تسويق قطط")

def main():
    app = ApplicationBuilder().token(BOT_TOKEN).build()
    app.add_handler(CommandHandler("صيد", handle_sayd))
    print("البوت شغّال يا صيّاد!")
    app.run_polling()

if __name__ == '__main__':
    main()
