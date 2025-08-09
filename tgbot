import telebot
from telebot import types
import requests

# 🔐 Твой токен от BotFather
TOKEN = '7604002554:AAFgaQmrS5ltlwSNA6wH6vs7cHiQlGtX40k'
bot = telebot.TeleBot(TOKEN)

# 📌 Канал, на который должен быть подписан пользователь
CHANNEL_USERNAME = '@elysianglow'

# 📄 Ссылка на гайд
GUIDE_LINK = 'https://drive.google.com/file/d/1VHck8gijN32yAzKJucKeUsj9vXi1Oqpl/view?usp=sharing'

# 🔍 Проверка подписки
def is_subscribed(user_id):
    try:
        status = bot.get_chat_member(CHANNEL_USERNAME, user_id).status
        return status in ['member', 'creator', 'administrator']
    except Exception as e:
        print(f"Ошибка при проверке подписки: {e}")
        return False

# 💾 Сохраняем ID пользователя
def save_user(user_id):
    try:
        with open("users.txt", "a") as f:
            f.write(f"{user_id}\n")
        print(f"Сохранён новый пользователь: {user_id}")
    except Exception as e:
        print("Ошибка записи пользователя:", e)

# 👋 Приветственное сообщение с кнопками
@bot.message_handler(commands=['start'])
def send_welcome(message):
    keyboard = types.ReplyKeyboardMarkup(resize_keyboard=True)
    button_guide = types.KeyboardButton("📝 Гайд")
    keyboard.add(button_guide)

    # сохраняем ID пользователя
    save_user(message.chat.id)

    bot.send_message(
        message.chat.id,
        "Привет!🤍\nНажми на кнопку, чтобы получить гайд.",
        reply_markup=keyboard
    )

# 📘 Выдача гайда по нажатию кнопки
@bot.message_handler(func=lambda message: message.text == "📝 Гайд")
def send_guide(message):
    user_id = message.from_user.id
    print(f"Пользователь {user_id} нажал кнопку '📝 Гайд'")  # ⬅️ отладка в консоли

    if is_subscribed(user_id):
        bot.send_message(
            message.chat.id,
            f"Вот гайд \"ТОП-10 ошибок в домашнем уходе, которые вредят коже, и как их избежать\":\n\n{GUIDE_LINK}"
        )
    else:
        bot.send_message(
            message.chat.id,
            "Пожалуйста, подпишись на канал, чтобы получить доступ к гайду 🤍"
        )

# 🔹 Повтор команды Старт
@bot.message_handler(func=lambda message: message.text == "Старт")
def restart(message):
    send_welcome(message)

# 🏃 Запуск бота
print("Бот запущен...")
bot.polling(none_stop=True)
