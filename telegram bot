import logging
import os
from telegram import InlineKeyboardButton, InlineKeyboardMarkup, ReplyKeyboardMarkup, Update
from telegram.ext import (
    ApplicationBuilder,
    CommandHandler,
    ContextTypes,
    MessageHandler,
    filters,
)

# =========================
# CONFIG
# =========================
# ВАЖНО:
# 1) Не храни токен в коде.
# 2) Перед запуском задай переменную окружения TELEGRAM_BOT_TOKEN
# 3) Так как токен уже был отправлен в чат, лучше перевыпусти его через BotFather.

BOT_TOKEN = os.getenv("TELEGRAM_BOT_TOKEN")

CAFE_NAME = "Light Kava"
CAFE_TAGLINE = "Кава з історією"
ADDRESS = "м. Київ, вул. Олександра Олеся, 9, ЖК Варшавський"
WORK_HOURS = "Щодня 08:00–21:00"
INSTAGRAM_URL = "https://www.instagram.com/light.kava/"
MAPS_URL = "https://www.google.com/maps/place/light+kava/@50.5002029,30.4219364,17z"
PHONE = "+380 XX XXX XX XX"  # заміни на реальний номер

MENU_TEXT = """
☕ *Що у нас є*

• Еспресо
• Американо
• Капучино
• Лате
• Авторські напої
• Матча / чай
• Свіжоспечені круасани
• Десерти

Щоб бот був корисним для гостей, сюди краще підставити реальне меню та ціни.
"""

ABOUT_TEXT = f"""
*{CAFE_NAME}* — {CAFE_TAGLINE}

📍 Адреса: {ADDRESS}
🕘 Графік: {WORK_HOURS}
✨ Формат: авторська кава, круасани, десерти
🔌 Перевага: стабільна робота, світло є завжди
"""

FAQ = {
    "адрес": f"Ми знаходимось тут: {ADDRESS}",
    "где": f"Ми знаходимось тут: {ADDRESS}",
    "как найти": f"Ось адреса: {ADDRESS}\n\nМаршрут: {MAPS_URL}",
    "часы": f"Ми працюємо так: {WORK_HOURS}",
    "время": f"Ми працюємо так: {WORK_HOURS}",
    "график": f"Ми працюємо так: {WORK_HOURS}",
    "меню": "Надсилаю короткий список позицій 👇",
    "кофе": "У нас є класика та авторські напої ☕",
    "десерт": "Так, у нас є десерти та свіжа випічка 🍰",
    "круассан": "Так, є свіжоспечені круасани 🥐",
    "инст": f"Наш Instagram: {INSTAGRAM_URL}",
    "instagram": f"Наш Instagram: {INSTAGRAM_URL}",
    "свет": "Так, ми працюємо стабільно — світло є завжди ⚡",
}

logging.basicConfig(
    format="%(asctime)s - %(name)s - %(levelname)s - %(message)s",
    level=logging.INFO,
)
logger = logging.getLogger(__name__)


def main_keyboard() -> ReplyKeyboardMarkup:
    keyboard = [
        ["📍 Адреса", "🕘 Графік"],
        ["☕ Меню", "📷 Instagram"],
        ["🗺 Як нас знайти", "📞 Контакти"],
    ]
    return ReplyKeyboardMarkup(keyboard, resize_keyboard=True)


def links_keyboard() -> InlineKeyboardMarkup:
    return InlineKeyboardMarkup(
        [
            [InlineKeyboardButton("Instagram", url=INSTAGRAM_URL)],
            [InlineKeyboardButton("Google Maps", url=MAPS_URL)],
        ]
    )


async def start(update: Update, context: ContextTypes.DEFAULT_TYPE) -> None:
    text = (
        f"Привіт! Я бот кав'ярні *{CAFE_NAME}* ☕\n\n"
        f"{CAFE_TAGLINE}\n\n"
        "Я можу підказати:\n"
        "• адресу\n"
        "• графік роботи\n"
        "• що є в меню\n"
        "• як нас знайти\n"
        "• де наш Instagram\n"
    )
    await update.message.reply_text(
        text,
        parse_mode="Markdown",
        reply_markup=main_keyboard(),
    )


async def help_command(update: Update, context: ContextTypes.DEFAULT_TYPE) -> None:
    await update.message.reply_text(
        "Команди:\n"
        "/start — запуск\n"
        "/about — про кав'ярню\n"
        "/menu — коротке меню\n"
        "/hours — графік\n"
        "/address — адреса\n"
        "/links — Instagram і Google Maps",
        reply_markup=main_keyboard(),
    )


async def about(update: Update, context: ContextTypes.DEFAULT_TYPE) -> None:
    await update.message.reply_text(ABOUT_TEXT, parse_mode="Markdown", reply_markup=links_keyboard())


async def menu(update: Update, context: ContextTypes.DEFAULT_TYPE) -> None:
    await update.message.reply_text(MENU_TEXT, parse_mode="Markdown", reply_markup=main_keyboard())


async def hours(update: Update, context: ContextTypes.DEFAULT_TYPE) -> None:
    await update.message.reply_text(f"🕘 {WORK_HOURS}", reply_markup=main_keyboard())


async def address(update: Update, context: ContextTypes.DEFAULT_TYPE) -> None:
    await update.message.reply_text(
        f"📍 {ADDRESS}\n\nВідкрити маршрут: {MAPS_URL}",
        reply_markup=links_keyboard(),
    )


async def links(update: Update, context: ContextTypes.DEFAULT_TYPE) -> None:
    await update.message.reply_text(
        "Ось наші посилання 👇",
        reply_markup=links_keyboard(),
    )


async def contacts(update: Update, context: ContextTypes.DEFAULT_TYPE) -> None:
    await update.message.reply_text(
        f"📞 Телефон: {PHONE}\n📷 Instagram: {INSTAGRAM_URL}",
        reply_markup=main_keyboard(),
    )


async def text_router(update: Update, context: ContextTypes.DEFAULT_TYPE) -> None:
    user_text = (update.message.text or "").strip().lower()

    # Кнопки
    if user_text == "📍 адреса":
        await address(update, context)
        return
    if user_text == "🕘 графік":
        await hours(update, context)
        return
    if user_text == "☕ меню":
        await menu(update, context)
        return
    if user_text == "📷 instagram":
        await update.message.reply_text(f"Наш Instagram: {INSTAGRAM_URL}", reply_markup=links_keyboard())
        return
    if user_text == "🗺 як нас знайти":
        await address(update, context)
        return
    if user_text == "📞 контакти":
        await contacts(update, context)
        return

    # Простий FAQ по ключових словах
    for key, value in FAQ.items():
        if key in user_text:
            await update.message.reply_text(value, reply_markup=main_keyboard())
            if key == "меню":
                await menu(update, context)
            return

    await update.message.reply_text(
        "Поки що я відповідаю на базові питання 😊\n"
        "Спробуй написати: адреса, графік, меню, інстаграм, як знайти",
        reply_markup=main_keyboard(),
    )


async def error_handler(update: object, context: ContextTypes.DEFAULT_TYPE) -> None:
    logger.exception("Exception while handling an update:", exc_info=context.error)


def build_app():
    if not BOT_TOKEN:
        raise RuntimeError("Не знайдено TELEGRAM_BOT_TOKEN у змінних оточення")

    app = ApplicationBuilder().token(BOT_TOKEN).build()

    app.add_handler(CommandHandler("start", start))
    app.add_handler(CommandHandler("help", help_command))
    app.add_handler(CommandHandler("about", about))
    app.add_handler(CommandHandler("menu", menu))
    app.add_handler(CommandHandler("hours", hours))
    app.add_handler(CommandHandler("address", address))
    app.add_handler(CommandHandler("links", links))
    app.add_handler(CommandHandler("contacts", contacts))
    app.add_handler(MessageHandler(filters.TEXT & ~filters.COMMAND, text_router))
    app.add_error_handler(error_handler)

    return app


if __name__ == "__main__":
    application = build_app()
    print("Bot is running...")
    application.run_polling()
