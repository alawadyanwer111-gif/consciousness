import telebot
from telebot import types
import random

# ضع التوكن الخاص بك هنا
API_TOKEN = '8289818503:AAGZxB4MWA2xVwSSCpHOGvo3TpKa2k0T2zc'
bot = telebot.TeleBot(API_TOKEN)

# قائمة رسائل متغيرة (تتغير في كل مرة)
messages_list = [
    "هلا.. بداية جديدة اليوم 🌿 هل حافظت على نفسك؟",
    "يوم جديد! هل كنت فخوراً بأفعالك اليوم؟",
    "مساء الخير.. هل أنجزت هدفك لهذا اليوم؟"
]

@bot.message_handler(commands=['start', 'send'])
def send_question(message):
    # اختيار رسالة عشوائية من القائمة
    text = random.choice(messages_list)
    
    # إنشاء الأزرار
    markup = types.InlineKeyboardMarkup()
    item_yes = types.InlineKeyboardButton("نعم", callback_data='yes')
    item_no = types.InlineKeyboardButton("لا", callback_data='no')
    markup.add(item_yes, item_no)
    
    bot.send_message(message.chat.id, text, reply_markup=markup)

# التعامل مع ضغطات الأزرار
@bot.callback_query_handler(func=lambda call: True)
def callback_inline(call):
    if call.data == "yes":
        bot.edit_message_text(chat_id=call.message.chat.id, 
                             message_id=call.message.message_id, 
                             text="أحسنت! استمر يا بطل ✨")
    elif call.data == "no":
        bot.edit_message_text(chat_id=call.message.chat.id, 
                             message_id=call.message.message_id, 
                             text="لا بأس، غداً فرصة جديدة للتحسن 💪")

bot.polling()
