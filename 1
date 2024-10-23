import random
import json
import time
import asyncio
from telegram import Update, InlineKeyboardButton, InlineKeyboardMarkup
from telegram.ext import ApplicationBuilder, CommandHandler, CallbackQueryHandler, ContextTypes

DATA_FILE = 'scores.json'
REFERRALS_FILE = 'referrals.json'
BASE_COOLDOWN_PERIOD = 7270 # 2 часов
GAME_COOLDOWN_PERIOD = 86400  # 24 часа
ADMIN_ID = 5584706559
BONUS_TDC = 300
CHANNEL_ID = "@dachatoken"

items = ["Помидоры", "Огурцы", "Клубника","Камни","Блум Поинты","Хомяки", "Картошка", "Вишня", "Черешня"]
game_messages = ["Кручу...", "Мучу...", "Туда-сюда..."]

def load_scores():
    try:
        with open(DATA_FILE, 'r') as f:
            return json.load(f)
    except FileNotFoundError:
        return {}

def save_scores(scores):
    with open(DATA_FILE, 'w') as f:
        json.dump(scores, f)

def load_referrals():
    try:
        with open(REFERRALS_FILE, 'r') as f:
            return json.load(f)
    except FileNotFoundError:
        return {}

def save_referrals(referrals):
    with open(REFERRALS_FILE, 'w') as f:
        json.dump(referrals, f)

scores = load_scores()
referrals = load_referrals()
user_data = {}

async def start(update: Update, context: ContextTypes.DEFAULT_TYPE) -> None:
    user_id = update.message.from_user.id
    referral_link = f"https://t.me/{context.bot.username}?start={user_id}"

    keyboard = [
        [InlineKeyboardButton("🔨 Собрать урожай", callback_data='mine')],
        [InlineKeyboardButton("💰 Проверить заначку", callback_data='score')],
        [InlineKeyboardButton("📊 Таблица лидеров", callback_data='leaderboard')],
        [InlineKeyboardButton("🎲 Делать ставку", callback_data='game')],
        [InlineKeyboardButton("ℹ️ Сотрудничество", callback_data='about')]
    ]

    # Добавляем кнопку админ панели только если пользователь является администратором
    if user_id == ADMIN_ID:
        keyboard.append([InlineKeyboardButton("👨‍💼 Админ панель", callback_data='admin')])

    reply_markup = InlineKeyboardMarkup(keyboard)

    await update.message.reply_text(
        f'Это твоя Дача! Используй кнопки для навигации.\n\n'
        f'🚀 Пригласи Тракториста на дачу с ним быстрее вы оба получите по 300 TDC!\nТвоя реферальная ссылка: {referral_link}',
        reply_markup=reply_markup
    )

async def button_handler(update: Update, context: ContextTypes.DEFAULT_TYPE) -> None:
    query = update.callback_query
    await query.answer()

    if query.data == 'mine':
        await mine(update, context)
    elif query.data == 'game':
        await game(update, context)
    elif query.data == 'score':
        await get_score(update, context)
    elif query.data == 'leaderboard':
        await leaderboard(update, context)
    elif query.data == 'admin':
        await admin_panel(update, context)
    elif query.data == 'about':
        await query.message.reply_text("Пиши https://t.me/officialtokendacha")
    elif query.data == 'admin_leaderboard':
        await admin_leaderboard(update, context)
    elif query.data == 'reset_scores':
        await reset_scores(update, context)

async def mine(update: Update, context: ContextTypes.DEFAULT_TYPE) -> None:
    user_id = update.callback_query.from_user.id
    chat_member = await context.bot.get_chat_member(CHANNEL_ID, user_id)

    if chat_member.status not in ['member', 'administrator', 'creator']:
        message = await update.callback_query.message.reply_text(f'Для того чтобы собрать урожай, подпишитесь на наш канал: t.me/dachatoken')
        await asyncio.sleep(2)
        await context.bot.delete_message(chat_id=update.effective_chat.id, message_id=message.message_id)
        return

    current_time = time.time()

    if user_id not in user_data:
        user_data[user_id] = {'last_time': 0, 'last_game_time': 0}

    last_mine_time = user_data[user_id]['last_time']

    if current_time - last_mine_time < BASE_COOLDOWN_PERIOD:
        remaining_time = int((BASE_COOLDOWN_PERIOD - (current_time - last_mine_time)) / 3600)
        message = await update.callback_query.message.reply_text(f'⏳ Вы уже собрали урожай. Приезжайте через {remaining_time} ч.')
        await asyncio.sleep(2)
        await context.bot.delete_message(chat_id=update.effective_chat.id, message_id=message.message_id)
        return

    user_data[user_id]['last_time'] = current_time

    action_messages = []
    for _ in range(3):
        item = random.choice(items)
        message = await update.callback_query.message.reply_text(f'🌱 {item}...')
        action_messages.append(message)
        await asyncio.sleep(1)

    await asyncio.sleep(1)
    for message in action_messages:
        try:
            await context.bot.delete_message(chat_id=update.effective_chat.id, message_id=message.message_id)
        except Exception as e:
            print(f"Ошибка при удалении сообщений: {e}")

    mined_points = round(random.uniform(0.1, 30), 1)
    scores[user_id] = scores.get(user_id, 0) + mined_points
    save_scores(scores)

    result_message = await update.callback_query.message.reply_text(f' ✅ Вы собрали {mined_points} TDC! На Даче: {scores[user_id]}')
    await asyncio.sleep(2)
    try:
        await context.bot.delete_message(chat_id=update.effective_chat.id, message_id=result_message.message_id)
    except Exception as e:
        print(f"Ошибка при удалении сообщения: {e}")

async def game(update: Update, context: ContextTypes.DEFAULT_TYPE) -> None:
    user_id = update.callback_query.from_user.id
    current_time = time.time()

    if user_id not in user_data:
        user_data[user_id] = {'last_time': 0, 'last_game_time': 0}

    last_game_time = user_data[user_id]['last_game_time']

    if current_time - last_game_time < GAME_COOLDOWN_PERIOD:
        remaining_time = int((GAME_COOLDOWN_PERIOD - (current_time - last_game_time)) / 3600)
        message = await update.callback_query.message.reply_text(f'⏳ Так и без штанов останешься. Приходи этак через {remaining_time} ч.')
        await asyncio.sleep(2)
        await context.bot.delete_message(chat_id=update.effective_chat.id, message_id=message.message_id)
        return

    bet_amount = 1000
    if scores.get(user_id, 0) < bet_amount:
        message = await update.callback_query.message.reply_text('❗ Хахаха, да это не смешно. Мало TDC для ставок. Требуется минимум 1000 TDC.')
        await asyncio.sleep(2)
        await context.bot.delete_message(chat_id=update.effective_chat.id, message_id=message.message_id)
        return

    user_data[user_id]['last_game_time'] = current_time
    scores[user_id] -= bet_amount
    save_scores(scores)

    await update.callback_query.message.reply_text("🎲 Ну щяс попрёт...")

    for game_message in game_messages:
        msg = await update.callback_query.message.reply_text(game_message)
        await asyncio.sleep(1)
        try:
            await context.bot.delete_message(chat_id=update.effective_chat.id, message_id=msg.message_id)
        except Exception as e:
            print(f"Ошибка при удалении сообщения: {e}")

    win = random.choice([True, False])
    if win:
        winnings = random.randint(1, 2000)
        scores[user_id] += winnings
        save_scores(scores)
        result_message = await update.callback_query.message.reply_text(f' ✅ Вот так дааааа! {winnings} TDC! Ваш текущий баланс: {scores[user_id]}')
        await asyncio.sleep(2)
        await context.bot.delete_message(chat_id=update.effective_chat.id, message_id=result_message.message_id)
    else:
        result_message = await update.callback_query.message.reply_text('❌ Увы, всё соседу.')
        await asyncio.sleep(2)
        await context.bot.delete_message(chat_id=update.effective_chat.id, message_id=result_message.message_id)

async def get_score(update: Update, context: ContextTypes.DEFAULT_TYPE) -> None:
    user_id = update.callback_query.from_user.id
    score = scores.get(user_id, 0)
    message = await update.callback_query.message.reply_text(f'Ваш текущий счёт: {score}')
    await asyncio.sleep(2)
    await context.bot.delete_message(chat_id=update.effective_chat.id, message_id=message.message_id)

async def admin_panel(update: Update, context: ContextTypes.DEFAULT_TYPE) -> None:
    user_id = update.callback_query.from_user.id
    keyboard = [
        [InlineKeyboardButton("📊 Таблица лидеров", callback_data='admin_leaderboard')],
        [InlineKeyboardButton("❌ Удалить всех игроков", callback_data='reset_scores')]
    ]
    reply_markup = InlineKeyboardMarkup(keyboard)

    await update.callback_query.message.reply_text("👨‍💼 Админ панель", reply_markup=reply_markup)

async def admin_leaderboard(update: Update, context: ContextTypes.DEFAULT_TYPE) -> None:
    sorted_scores = sorted(scores.items(), key=lambda x: x[1], reverse=True)
    leaderboard_message = "📊 Таблица лидеров (Админ):\n"
    
    for i, (user_id, score) in enumerate(sorted_scores[:10], start=1):
        user = await context.bot.get_chat(user_id)
        username = user.username if user.username else f"User ID: {user_id}"
        leaderboard_message += f"{i}. @{username}: {score} TDC\n"

    await update.callback_query.message.reply_text(leaderboard_message)

async def leaderboard(update: Update, context: ContextTypes.DEFAULT_TYPE) -> None:
    sorted_scores = sorted(scores.items(), key=lambda x: x[1], reverse=True)
    leaderboard_message = "📊 Таблица лидеров:\n"
    
    for i, (user_id, score) in enumerate(sorted_scores[:10], start=1):
        user = await context.bot.get_chat(user_id)
        username = user.username if user.username else f"User ID: {user_id}"
        leaderboard_message += f"{i}. @{username}: {score} TDC\n"

    await update.callback_query.message.reply_text(leaderboard_message)


async def reset_scores(update: Update, context: ContextTypes.DEFAULT_TYPE) -> None:
    scores.clear()
    save_scores(scores)
    await update.callback_query.message.reply_text("✅ Все счета были сброшены.")

app = ApplicationBuilder().token("7613309061:AAHj2mfGvhZr5liu3DJbhCJapgAsy4SzRzw").build()

app.add_handler(CommandHandler("start", start))
app.add_handler(CallbackQueryHandler(button_handler))

app.run_polling()

