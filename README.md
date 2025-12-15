# lab_2
# Цель работы
Изучить методы отправки и анализа HTTP-запросов с использованием инструментов telnet и curl, освоить базовую настройку и анализ работы HTTP-сервера nginx в качестве веб-сервера и обратного прокси, а также изучить и применить на практике концепции архитектурного стиля REST для создания веб-сервисов (API) на языке Python.

1. Анализ заголовка ContentType при запросе к главной странице vk.com.
   
2. API для "Список вакансий" (сущность: id, job_title, company).
   
3. Настроить Nginx как обратный прокси для Flask API.
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

