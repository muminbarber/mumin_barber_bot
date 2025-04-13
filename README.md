# mumin_barber_botfrom telegram import InlineKeyboardButton, InlineKeyboardMarkup, Update
from telegram.ext import ApplicationBuilder, CommandHandler, CallbackQueryHandler, ContextTypes

TOKEN = "SENING_BOT_TOKENING"  # o'zingning tokenni shu yerga qo'y

services = {
    "Soch olish": "50.000 so‘m — 1 soat",
    "Soch olish + Ukladka": "70.000 so‘m — 1 soat",
    "Yuz tozalash": "70.000 so‘m — 1 soat",
    "Kuyov soch turmak": "200.000 so‘m — 1:30 soat",
    "Bolalar sochi": "40.000 so‘m — 1 soat"
}

async def start(update: Update, context: ContextTypes.DEFAULT_TYPE):
    keyboard = [[InlineKeyboardButton(name, callback_data=name)] for name in services]
    reply_markup = InlineKeyboardMarkup(keyboard)
    await update.message.reply_text("Xizmat turini tanlang:", reply_markup=reply_markup)

async def button(update: Update, context: ContextTypes.DEFAULT_TYPE):
    query = update.callback_query
    await query.answer()
    service = query.data
    info = services[service]
    await query.edit_message_text(text=f"Siz tanladingiz: {service}\nNarxi va vaqti: {info}\n\nTez orada siz bilan bog‘lanamiz.")

app = ApplicationBuilder().token(TOKEN).build()

app.add_handler(CommandHandler("start", start))
app.add_handler(CallbackQueryHandler(button))

print("Bot ishlamoqda...")
app.run_polling()
