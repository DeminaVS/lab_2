# lab_2
# Цель работы
Изучить методы отправки и анализа HTTP-запросов с использованием инструментов telnet и curl, освоить базовую настройку и анализ работы HTTP-сервера nginx в качестве веб-сервера и обратного прокси, а также изучить и применить на практике концепции архитектурного стиля REST для создания веб-сервисов (API) на языке Python.

1. Анализ заголовка при запросе к главной странице vk.com.
   <img width="2075" height="568" alt="Снимок экрана 2025-12-17 015047" src="https://github.com/user-attachments/assets/da0f06c6-2ccd-46bb-aef2-51ad1b5170ba" />


2. API для "Список вакансий" (сущность: id, job_title, company).
   <img width="1280" height="394" alt="image" src="https://github.com/user-attachments/assets/bf15346a-60bc-4ede-9d97-254beebc4761" />

3. Настроить Nginx как обратный прокси для Flask API.
   <img width="1280" height="299" alt="image" src="https://github.com/user-attachments/assets/f2771e01-d690-45c1-a43b-e434a1576744" />

# Вариант 9
# Ход работы 
# Часть 1: Анализ заголовка ContentType при запросе к главной странице vk.com.
Создаем диреекторию проекта и переходим в нее. При помощи команды
```
sudo apt install curl -y
```
установим расширение для работы с веб-ресурсами через сетевые протоколы:

<img width="1838" height="904" alt="image" src="https://github.com/user-attachments/assets/b79f238a-cfbe-4a8f-8a77-14c9a5995678" />

Отправляем запрос с анализом заголовков

<img width="1923" height="641" alt="image" src="https://github.com/user-attachments/assets/a9d85114-3ebc-472c-a49f-a07c00a9aca9" />

# Часть 2: Разработка REST API для "Списка вакансий"
Создаем и активируем виртуальное окружение
```
sudo apt install python3-venv -y
```
```
python3 -m venv venv
```
```
source venv/bin/activate
```
<img width="1312" height="340" alt="image" src="https://github.com/user-attachments/assets/5679003c-444d-4460-b5ea-dc3a95720307" />

Теперь с ативным виртуальным окружением устанавливаем Flask

```
pip install Flask
```
<img width="1437" height="654" alt="image" src="https://github.com/user-attachments/assets/d5aa0179-5bfa-4868-84f2-fe60b6e40f34" />

Далее создаем файл app.py и запускаем API

```
python3 app.py
```
Сервер успешно запущен

<img width="1369" height="345" alt="image" src="https://github.com/user-attachments/assets/fe2145a7-fa34-434d-8900-5548d383adb0" />

Протестируем API в новом терминале:

```
curl -s http://127.0.0.1:5000/api/vacancies | jq
```
<img width="853" height="529" alt="image" src="https://github.com/user-attachments/assets/a682538e-f31b-43a4-9ac3-d1232cf43a33" />

Добавим новую вакансию

<img width="908" height="311" alt="image" src="https://github.com/user-attachments/assets/b3e80f58-df2d-457a-b417-074da40b00af" />

Финальная проверка

<img width="885" height="878" alt="image" src="https://github.com/user-attachments/assets/0e36b8fe-89ab-447a-93de-3c59749b1a1a" />

# Часть 3: Настройка Nginx как обратного прокси
Сначала произведем установку Nginx:

```
sudo apt install nginx -y
```
```
sudo systemctl start nginx
```
```
sudo systemctl enable nginx
```
<img width="1285" height="477" alt="image" src="https://github.com/user-attachments/assets/c8b04c86-8aec-486d-9bde-2d7960d5c43f" />

Открываем конфигурационный файл:

```
sudo nano /etc/nginx/sites-available/default
```
<img width="545" height="417" alt="image" src="https://github.com/user-attachments/assets/caf5359f-8489-49f2-bc8b-1a824e830f2e" />

Далее находим блок location / { ... }, он отвечает за главную страницу, и добавляем следующий блок под ним:

```
location /api/ {
    # Перенаправляем запрос на наш Flask-сервер
    proxy_pass http://127.0.0.1:5000;

    # Передаем важные заголовки
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto $scheme;
}
```
Проверяем, нет ли в наших файлах синтаксических ошибок

```
sudo nginx -t
```
И перезапускаем Nginx, чтобы изменения были применены:

<img width="1129" height="92" alt="image" src="https://github.com/user-attachments/assets/8bb70651-885d-4fc3-9a8f-d08d7fa68721" />

Проверим, что Nginx работает:

```
curl http://localhost
```
Получаем результат:

<img width="819" height="801" alt="image" src="https://github.com/user-attachments/assets/27eed0e7-dd84-4021-bace-014874fcc756" />

Теперь проверим, что запросы к /api/vacancies перенаправляются на Flask:

```
curl -i http://localhost/api/vacancies
```
Первый запрос — "Холодный" кеш (MISS-EXPIRED):

<img width="817" height="787" alt="image" src="https://github.com/user-attachments/assets/fad9e2c4-5d1e-4f7e-bdec-ea2871da6766" />

Второй запрос — "Горячий" кеш (HIT):

<img width="812" height="632" alt="image" src="https://github.com/user-attachments/assets/e6c5f2b9-d48f-4b88-8813-fe07277b7501" />

Попробуем добавить вакансию через Nginx:

```
curl -X POST http://localhost/api/vacancies -H "Content-Type: application/json" -d '{"job_title":"Python development", "company":"sber", "salary":250000}'
```
Финальная проверка изменений и обратим внимание на логи в терминале Flask:

<img width="735" height="931" alt="image" src="https://github.com/user-attachments/assets/02fcb609-e4ba-49c2-81f9-8f8c52d0fc23" />

<img width="923" height="620" alt="image" src="https://github.com/user-attachments/assets/ab8133a6-fd6f-4f8b-8830-eb41923f14a0" />

# Выводы
В ходе работы мы смогли изучить методы отправки и анализа HTTP-запросов с использованием инструментов telnet и curl, освоить базовую настройку и анализ работы HTTP-сервера nginx в качестве веб-сервера и обратного прокси, а также изучить и применить на практике концепции архитектурного стиля REST для создания веб-сервисов (API) на языке Python.
