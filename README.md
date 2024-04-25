обязательно к прочтению

import g4f
from g4f.Provider import (
    GeekGpt,
    Liaobots,
    Phind,
    Raycast,
    RetryProvider)
from g4f.client import Client
import nest_asyncio
nest_asyncio.apply()

client = Client(
    provider = RetryProvider([
            g4f.Provider.Liaobots,
            g4f.Provider.GeekGpt,
            g4f.Provider.Phind,
            g4f.Provider.Raycast
    ])
  )
chat_history = [{"role": "user", "content": 'Отвечай на русском языке'}]


def send_request(message):
    global chat_history
    print(chat_history)
    otvet=[]
    #global otvet
    chat_history[0]["content"] += message + " "
    #print(chat_history)
    

    try:
        response = g4f.ChatCompletion.create(
        model=g4f.models.gpt_4,
        messages=chat_history
#         response = g4f.ChatCompletion.create(
#         model=g4f.models.default,
#         provider=g4f.Provider.OpenaiChat,
#         messages=chat_history
    )
    except Exception as err:
        print("Все провайдеры не отвечают, попробуйте пойзже")
#     print(response)
#     chat_history[0]["content"] += response + " "
#         #time.sleep(120)
#         response = g4f.ChatCompletion.create(
#         model=g4f.models.default,
#         provider=g4f.Provider.OpenaiChat,
#         messages=chat_history
#     )
    #otvet = []    
    #otvet = response.split('\n\n')
    #print(otvet[0]) 

    #otvet = response.split('\n\n')
    #for otvet in otvet:
        #bot.send_message(message.chat.id, 0)
        #print(i)
        #print(otvet[i])
        
#     print(chat_history) 
    print(response)
    return (response)

    chat_history[0]["content"] += response + " "
    
    
    
    
def img_sell(promp):
    prompt = GoogleTranslator(source='auto', target='en').translate(promp) + "  , --W 256 --H 256 "

    #prompt = "An astronaut riding a green horse"

    images = pipe(prompt=prompt).images[0]
    
    return(images) 

                            

    
@bot.callback_query_handler(func=lambda call: True)
def callback_inline(call):  
    if call.message:
        if call.data == "button1":
            bot.send_message(call.message.chat.id, 'Все по кайфу')
            chat_history[0]["content"] = ""
#             bot.send_message(call.message.chat.id, "Вы нажали на первую кнопку.")
#         if call.data == "button2":
#             bot.send_message(call.message.chat.id, "Вы нажали на вторую кнопку.")
 
@bot.message_handler(commands=['start'])
def start(message):
    
    keyboa = types.InlineKeyboardMarkup()
    #добавляем на нее две кнопки
    button1 = types.InlineKeyboardButton(text="завершить чат", callback_data="button1")
    #button2 = types.InlineKeyboardButton(text="Кнопка 2", callback_data="button2")
    keyboa.add(button1)
    #keyboard.add(button2)
    
    
    markup = types.ReplyKeyboardMarkup(resize_keyboard=True)
    btn1 = types.KeyboardButton("Напиши сказку")
#     btn2 = types.KeyboardButton("Расскажи сказку")
#     btn3 = types.KeyboardButton("Настройки сказки")
#     btn4 = types.KeyboardButton("какой диаметр луны")
    markup.add(btn1)
    
    bot.send_message(message.chat.id, 'Привет! Можешь спрашивать меня! Что тебя интересует?', reply_markup=markup)

@bot.message_handler(commands=['new'])
def new(message):
    bot.send_message(message.chat.id, 'Ну, держи) ', reply_markup=keyboa)
    chat_history[0]["content"] = "Привет, сочини самостоятельно сказку"
    otvet = send_request(message.text).split('\n\n')  
    for i in otvet:
        bot.send_photo(message.chat.id, img_sell(i), i)  
    bot.send_message(message.chat.id, 'Конец', reply_markup=keyboa)
    chat_history[0]["content"] = ""
    #bot.send_message(message.chat.id, send_request(message.text))
    #print(otvet[0]) 
    
@bot.message_handler(commands=['n1'])
def start(message):
    bot.send_message(message.chat.id, 'Забыл')
    chat_history[0]["content"] = ""
    #print(otvet[0], 'lol') 
    #bot.send_photo(message.chat.id, open(/kaggle/working/...), 'text')
    
    #audio = open(/kaggle/working/...)
    #bot.send_audio(message.chat.id, audio)
    #audio.close()
    
    
    
    #print(response) 
    #print(type(response)) 
#
keyboa = types.InlineKeyboardMarkup()
    #добавляем на нее две кнопки
button1 = types.InlineKeyboardButton(text="Завершить чат", callback_data="button1")
    #button2 = types.InlineKeyboardButton(text="Кнопка 2", callback_data="button2")
keyboa.add(button1) 
    
@bot.message_handler(content_types=['text'])
def get_text_messages(message):
    if(message.text == "Напиши сказку"):
        bot.send_message(message.chat.id, 'Ну, держи) ')
        chat_history[0]["content"] = ""
        chat_history[0]["content"] = "Привет, сочини самостоятельно большую сказку с диалогами."
        
        
   
    bot.send_message(message.chat.id, send_request(message.text), reply_markup=keyboa)
    

    
bot.polling(none_stop=True, interval=0)

Вот кот, Разбирайтесь сами.
