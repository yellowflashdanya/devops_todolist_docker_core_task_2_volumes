# Інструкції для запуску MySQL і Python App контейнерів

### 1. **Підготовка Dockerfile для MySQL**

Створіть файл `Dockerfile.mysql` для налаштування бази даних MySQL за допомогою офіційного образу MySQL.

**Dockerfile.mysql:**

```dockerfile
# Використовуємо офіційний образ MySQL
FROM mysql:latest

# Встановлюємо змінні середовища для MySQL
ENV MYSQL_ROOT_PASSWORD=rootpassword
ENV MYSQL_DATABASE=app_db
ENV MYSQL_USER=app_user
ENV MYSQL_PASSWORD=1234

# Відкриваємо порт MySQL (за замовчуванням 3306)
EXPOSE 3306
```

### 2. **Building the Image of MySQL**

Для створення образу MySQL з файлу Dockerfile.mysql скористайтеся командою:

```bash
docker build -f Dockerfile.mysql -t mysql-local:1.0.0 .
```

Це створить образ з тегом mysql-local:1.0.0.

### 3. **Запуск контейнера MySQL з прикріпленим томом**

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

### 4. **Як отримати доступ до додатку через браузер**

```arduino
http://localhost:порт_додатку
```

Замість порт_додатку вкажіть порт, який ви відкрили в Docker при запуску контейнера Python додатку (наприклад, 8000, якщо ви використовуєте Django за замовчуванням).

- Примітки

MySQL Image: https://hub.docker.com/repository/docker/danyakube/mysql-local/general

Python App Image: https://hub.docker.com/repository/docker/danyakube/todoapp/general
