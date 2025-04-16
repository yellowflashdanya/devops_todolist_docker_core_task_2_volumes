# Use the official MySQL image from Docker Hub.

FROM mysql:latest

# Set environment variables for MySQL

ENV MYSQL_ROOT_PASSWORD=rootpassword
ENV MYSQL_DATABASE=app_db
ENV MYSQL_USER=app_user
ENV MYSQL_PASSWORD=1234

# Expose the default MySQL port

EXPOSE 3306

2. Building the Image of MySQL
   Для створення образу MySQL з файлу Dockerfile.mysql скористайтеся командою:

```bash
docker build -f Dockerfile.mysql -t mysql-local:1.0.0 .
```

Це створить образ з тегом mysql-local:1.0.0.

3. Запуск контейнера MySQL з прикріпленим томом
   Тепер ви можете запустити контейнер для MySQL з прикріпленим томом для зберігання даних:

```bash
docker run -d \
  --name mysql-container \
  -e MYSQL_ROOT_PASSWORD=rootpassword \
  -e MYSQL_DATABASE=app_db \
  -e MYSQL_USER=app_user \
  -e MYSQL_PASSWORD=1234 \
  -v mysql-data:/var/lib/mysql \
  -p 3306:3306 \
  mysql-local:1.0.0
```

Пояснення:

-v mysql-data:/var/lib/mysql: Прикріпляє том з назвою mysql-data для зберігання даних MySQL. Це дозволяє зберігати дані між перезапусками контейнера.

-p 3306:3306: Відкриває порт 3306 для доступу до MySQL з хостової машини.

4. Запуск Python App контейнера та підключення до MySQL
   Перш ніж запускати Python App контейнер, оновіть конфігурацію підключення до бази даних в Python додатку. Вкажіть IP-адресу контейнера MySQL. Для цього можна використовувати ім'я контейнера замість IP, оскільки Docker автоматично налаштовує мережу між контейнерами.

Оновлення конфігурації Python додатку:
Змініть значення HOST у конфігурації вашого Python додатку (наприклад, для Django в settings.py):

```python
DATABASES = {
    'default': {
        'ENGINE': 'mysql.connector.django',
        'NAME': 'app_db',
        'USER': 'app_user',
        'PASSWORD': '1234',
        'HOST': 'mysql-container',  # Ім'я контейнера MySQL
        'PORT': '3306',  # Порт MySQL
    }
}
```

Запуск контейнера Python App:
Для запуску контейнера додатку (наприклад, якщо у вас є Dockerfile для додатку Python):

```bash
docker build -t python-app:latest .
docker run -d --name python-app --link mysql-container:mysql -p 8000:8000 python-app:latest
```

Пояснення:

--link mysql-container:mysql: Встановлює зв'язок між контейнерами. mysql-container — це ім'я контейнера MySQL, а mysql — псевдонім для доступу до цього контейнера в Python додатку.

-p 8000:8000: Відкриває порт 8000 для доступу до вашого Python додатку (залежно від того, який порт відкрито у вашому Dockerfile для Python додатку).

5. Як отримати доступ до додатку через браузер
   Відкрийте браузер і перейдіть за адресою:

```arduino
http://localhost:8000
```

6. Посилання на репозиторії на Docker Hub
   MySQL Image на Docker Hub: https://hub.docker.com/repository/docker/danyakube/mysql-local/general

Python App Image на Docker Hub: https://hub.docker.com/repository/docker/danyakube/todoapp/general
