# lab_2
# Цель работы
Изучить методы отправки и анализа HTTP-запросов с использованием инструментов telnet и curl, освоить базовую настройку и анализ работы HTTP-сервера nginx в качестве веб-сервера и обратного прокси, а также изучить и применить на практике концепции архитектурного стиля REST для создания веб-сервисов (API) на языке Python.

1. Анализ заголовка ContentType при запросе к главной странице vk.com.
   
2. API для "Список вакансий" (сущность: id, job_title, company).
   
3. Настроить Nginx как обратный прокси для Flask API.
# Вариант 9
# Ход работы 
# Часть 1: Анализ заголовка ContentType при запросе к главной странице vk.com.
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

