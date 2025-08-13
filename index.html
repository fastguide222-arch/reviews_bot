import logging
from aiogram import Bot, Dispatcher, types
from aiogram.contrib.fsm_storage.memory import MemoryStorage
from aiogram.dispatcher import FSMContext
from aiogram.dispatcher.filters.state import State, StatesGroup
from aiogram.utils import executor

# ===== НАСТРОЙКИ =====
API_TOKEN = "8496471458:AAG-O5Eli7krQx9FKHmpr5jyLnCV5AvP3c0"  # Твой токен бота
ADMIN_ID = 8458994953  # Твой Telegram ID

CHANNELS = {
    "-1002526285031": "Канал 1",
    "-1002716220445": "Канал 2",
    "-1002651825180": "Канал 3",
    "-1002770818048": "Канал 4",
    "-1002622247826": "Канал 5",
}

logging.basicConfig(level=logging.INFO)

bot = Bot(token=API_TOKEN, parse_mode="Markdown")
dp = Dispatcher(bot, storage=MemoryStorage())

# ===== СОСТОЯНИЯ =====
class ReviewForm(StatesGroup):
    rating = State()
    channel = State()
    review_text = State()

# ===== ФУНКЦИИ =====
def progress(step):
    steps = ["Оценка", "Выбор канала", "Проверка", "Отзыв"]
    return " ➡ ".join(
        f"[{s}]" if i == step else s
        for i, s in enumerate(steps)
    )

def rating_keyboard():
    kb = types.ReplyKeyboardMarkup(resize_keyboard=True)
    kb.add("⭐ 1", "⭐⭐ 2", "⭐⭐⭐ 3", "⭐⭐⭐⭐ 4", "⭐⭐⭐⭐⭐ 5")
    return kb

# ===== ХЕНДЛЕРЫ =====
@dp.message_handler(commands=["start"])
async def cmd_start(message: types.Message):
    await message.answer(
        f"{progress(0)}\nВыберите оценку от 1 до 5:",
        reply_markup=rating_keyboard()
    )
    await ReviewForm.rating.set()

@dp.message_handler(state=ReviewForm.rating)
async def set_rating(message: types.Message, state: FSMContext):
    if not any(char.isdigit() for char in message.text):
        return await message.reply("Пожалуйста, выберите оценку с клавиатуры.")

    rating = int("".join(filter(str.isdigit, message.text)))
    if rating < 1 or rating > 5:
        return await message.reply("Оценка должна быть от 1 до 5.")

    await state.update_data(rating=rating)

    kb = types.ReplyKeyboardMarkup(resize_keyboard=True)
    for cid, name in CHANNELS.items():
        kb.add(name)

    await message.answer(
        f"{progress(1)}\nВыберите канал:",
        reply_markup=kb
    )
    await ReviewForm.channel.set()

@dp.message_handler(state=ReviewForm.channel)
async def set_channel(message: types.Message, state: FSMContext):
    channel_name = message.text.strip()
    for cid, name in CHANNELS.items():
        if name == channel_name:
            await state.update_data(channel_id=cid, channel_name=name)
            await message.answer(f"{progress(2)}\nПроверяю вашу подписку на {name}...")

            try:
                member = await bot.get_chat_member(cid, message.from_user.id)
            except Exception:
                await message.answer("Не удалось проверить подписку. Попробуйте позже.")
                await state.finish()
                return

            if member.status in ["member", "administrator", "creator"]:
                template = f"**Отзыв для канала \"{name}\"**\n"
                await state.update_data(review_template=template)
                await message.answer(
                    f"{progress(3)}\nВы подписаны ✅\n"
                    f"Пожалуйста, напишите свой отзыв начиная с этой строки:\n\n{template}"
                )
                await ReviewForm.review_text.set()
            else:
                await message.answer(
                    "❌ Вы не подписаны на канал. Подпишитесь и начните заново."
                )
                await state.finish()
            return

    await message.reply("Пожалуйста, выберите канал из списка.")

@dp.message_handler(state=ReviewForm.review_text)
async def set_review(message: types.Message, state: FSMContext):
    data = await state.get_data()
    template = data.get("review_template", "")

    text = message.text.strip()

    # Проверка на шаблон
    if not text.startswith(template):
        return await message.answer(
            f"⚠ Пожалуйста, начните отзыв с этой строки:\n\n{template}"
        )

    # Проверка длины текста без первой строки
    review_body = text[len(template):].strip()
    if len(review_body) < 70:
        return await message.answer(
            f"⚠ Ваш отзыв слишком короткий ({len(review_body)} символов). Минимум — 70.\n"
            f"Пожалуйста, допишите."
        )

    await state.update_data(review=text, review_body=review_body)

    rating = data['rating']
    channel_id = data['channel_id']
    channel_name = data['channel_name']

    review_message = (
        f"⭐ {rating} / {channel_name}\n"
        f"{text}\n"
        f"— {message.from_user.full_name}"
        + (f" (@{message.from_user.username})" if message.from_user.username else "")
    )

    if rating >= 4:
        await bot.send_message(channel_id, review_message, parse_mode="Markdown")
        await message.answer("Спасибо! Ваш отзыв опубликован ✅")
    else:
        await bot.send_message(ADMIN_ID, "❗ Получен отзыв с низкой оценкой:\n" + review_message, parse_mode="Markdown")
        await message.answer("Спасибо за ваш отзыв! Он будет рассмотрен администрацией.")

    await state.finish()

# ===== ЗАПУСК =====
if __name__ == "__main__":
    executor.start_polling(dp, skip_updates=True)
