CLIENT WARS — быстрый запуск

1. Откройте index.html в браузере. В локальном режиме счётчики и профиль сохраняются на этом устройстве.
2. Чтобы два человека видели одну статистику с разных устройств, нужен бесплатный проект Supabase:
   https://supabase.com/dashboard
   Создайте проект на Free plan (если доступен в вашем регионе/аккаунте).
3. В Supabase откройте SQL Editor и выполните:

create table if not exists public.rooms (
  room text primary key,
  state jsonb not null default '{"players":[],"history":[]}'::jsonb,
  updated_at timestamptz not null default now()
);

alter table public.rooms enable row level security;

create policy "public room read" on public.rooms
for select to anon using (true);

create policy "public room insert" on public.rooms
for insert to anon with check (true);

create policy "public room update" on public.rooms
for update to anon using (true) with check (true);

4. В Project Settings → API скопируйте Project URL и anon/public key.
   НЕ используйте service_role key.
5. Откройте CLIENT WARS → ⚙️ и вставьте URL, anon key и общий код комнаты.
   На втором устройстве введите те же три значения.
6. Данные будут синхронизироваться опросом облака примерно раз в 5 секунд.

ВАЖНО ПРО БЕЗОПАСНОСТЬ:
Это шуточный прототип для небольшой доверенной компании. Общий код комнаты — не пароль.
С этими простыми открытыми правилами любой, кто знает имя таблицы и anon key, может читать/менять данные.
Не храните личные или чувствительные сведения. Для публичного продукта нужны настоящая авторизация,
проверка прав на сервере и более строгие RLS-политики.

Публикация бесплатной статической страницы:
- Можно использовать GitHub Pages или Cloudflare Pages на бесплатном тарифе, если он доступен.
- Загрузите index.html в репозиторий/проект и включите статический хостинг.
- Никому не передавайте секретные ключи; в приложении используется только anon/public key.

Ограничение комнаты:
- Максимум 10 игроков в одной комнате. Один профиль создаётся на одном устройстве/браузере.
- Каждый участник должен использовать одинаковые Project URL, anon/public key и код комнаты.

Текущие функции:
- Никнейм и выбор аватара.
- Шесть категорий, плюс/минус.
- Личная статистика, батл, достижения, история.
- Локальное сохранение.
- Необязательное подключение Supabase.
