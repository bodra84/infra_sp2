# Инфраструктура для социальной сети "YaMDb"
### Описание:
Проект "YaMDb" собирает отзывы (Review) пользователей на произведения (Title). Произведения делятся на категории: "Книги", "Фильмы", "Музыка". Список категорий (Category) может быть расширен.
Сами произведения в YaMDb не хранятся, здесь нельзя посмотреть фильм или послушать музыку.
В каждой категории есть произведения: книги, фильмы или музыка. Например, в категории "Книги" могут быть произведения "Винни Пух и все-все-все" и "Марсианские хроники", а в категории "Музыка" — песня "Давеча" группы "Насекомые" и вторая сюита Баха. Произведению может быть присвоен жанр из списка предустановленных (например, "Сказка", "Рок" или "Артхаус"). Новые жанры может создавать только администратор.
Благодарные или возмущённые читатели оставляют к произведениям текстовые отзывы (Review) и выставляют произведению рейтинг.
Для работы проета разворачивается инфраструктура из трех docker контейнеров (web, nginx и DB). 
### Технологии
- Python  
- Django REST framework 
- Django ORM
- REST API 
- PostgreSQL
- Nginx 
- Gunicorn
- Docker

**Ресурсы API YaMDb:**

**AUTH**: аутентификация.

**USERS**: пользователи.

**TITLES**: произведения, к которым пишут отзывы (определённый фильм, книга или песенка).

**CATEGORIES**: категории (типы) произведений ("Фильмы", "Книги", "Музыка").

**GENRES**: жанры произведений. Одно произведение может быть привязано к нескольким жанрам.

**REVIEWS**: отзывы на произведения. Отзыв привязан к определённому произведению.

**COMMENTS**: комментарии к отзывам. Комментарий привязан к определённому отзыву.

# Пользовательские роли:
**Аноним** — может просматривать описания произведений, читать отзывы и комментарии.

**Аутентифицированный пользователь (user)** — может читать всё, как и Аноним, дополнительно может публиковать отзывы и ставить рейтинг произведениям (фильмам/книгам/песенкам), может комментировать чужие отзывы и ставить им оценки; может редактировать и удалять свои отзывы и комментарии.

**Модератор (moderator)** — те же права, что и у Аутентифицированного пользователя плюс право удалять и редактировать любые отзывы и комментарии.

**Администратор (admin)** — полные права на управление проектом и всем его содержимым. Может создавать и удалять произведения, категории и жанры. Может назначать роли пользователям.

**Администратор Django** — те же права, что и у роли Администратор.

# Регистрация пользователей:
Пользователь отправляет POST-запрос с параметром email на `/api/v1/auth/email/`.
YaMDB отправляет письмо с кодом подтверждения (confirmation_code) на адрес email (функция в разработке).
Пользователь отправляет POST-запрос с параметрами email и confirmation_code на `/api/v1/auth/token/`, в ответе на запрос ему приходит token (JWT-токен).
Эти операции выполняются один раз, при регистрации пользователя. В результате пользователь получает токен и может работать с API, отправляя этот токен с каждым запросом.

# Шаблон наполнения env-файла:
**SECRET_KEY** - ключ для криптографической подписи Django. Можно сгенерировать на сайте https://djecrety.ir/

**DB_ENGINE** - тип используемой базы данных. Укажите, что используете postgresql (django.db.backends.postgresql).

**DB_NAME** - имя базы данных.

**POSTGRES_USER** - логин для подключения к базе данных.

**POSTGRES_PASSWORD** - пароль для подключения к БД (установите свой).

**DB_HOST** - название сервиса (по умолчанию "db").

**DB_PORT** - порт для подключения к БД (по умолчанию "5432").

# Установка проекта:
1.Запустить сборку и запуск контейнеров:
```sh
docker-compose up -d --build
```
2.Выполнить миграции:
```sh
docker-compose exec web python manage.py makemigrations
docker-compose exec web python manage.py migrate
```
3.Выполнить сбор статических файлов:
```sh
docker-compose exec web python manage.py collectstatic --no-input
```
4.Создать суперпользователя (при необходимости):
```sh
docker-compose exec web python manage.py createsuperuser
```
5.Заполнить базу данных из файла **fixtures.json** (при необходимости):
Скопируйте файл fixtures.json в контейнер web в папку "api_yambd" и выполните команду для заполнения БД
```sh
docker-compose exec web python manage.py loaddata fixtures.json
```
**Поздравляю, проект готов к работе!**

**Пример:**

Для доступа к API необходимо получить токен:
Нужно выполнить POST-запрос http://localhost/api/v1/auth/jwt/create/ передав поля username и password. API вернет JWT-токен
Дальше, передав токен можно будет обращаться к методам, например:
/api/v1/posts/ (GET, POST, PUT, PATCH, DELETE)
При отправке запроса передавайте токен в заголовке Authorization: Bearer <токен>

# Авторы:

Вадим Белозеров

Давлат Файзиев

e-mail: bodra84@gmail.com

Алена Котова
