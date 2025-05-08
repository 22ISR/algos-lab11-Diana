# Использование API

## Задание

Разработать CLI-программу на Python, которая будет принимать запросы от пользователя, отправлять их на сервер OMDb API и выводить информацию о найденных фильмах.

## API

### [OMDB](https://www.omdbapi.com/)

API ключ `505480d7`

_Вас интересует запрос с ключем `s`_

### Технические требования

1. Используйте библиотеку requests для выполнения HTTP-запросов.
2. Программа должна работать в цикле и принимать множество запросов от пользователя.
3. Результаты поиска должны быть выведены в читаемом формате (например, "Название: Название, Год: Год, Тип: Тип").
4. Программа должна корректно обрабатывать ошибки при недоступности API и сообщать пользователю об этом.

## Пример работы

```bash
❯ py main.py
Введите название фильма (или напишите 'выход' чтобы завершить работу): Joker
Название: Joker, Год: 2019, Тип: movie
Название: Joker: Folie à Deux, Год: 2024, Тип: movie
Название: Batman Beyond: Return of the Joker, Год: 2000, Тип: movie
Название: Joker, Год: 2012, Тип: movie
Название: Mera Naam Joker, Год: 1970, Тип: movie
Название: Joker, Год: 2021–2024, Тип: series
Название: Joker, Год: 2016, Тип: movie
Название: The Joker Is Wild, Год: 1957, Тип: movie
Название: The People's Joker, Год: 2022, Тип: movie
Название: Batman vs Joker: Final Joke, Год: 2008, Тип: movie
Введите название фильма (или напишите 'выход' чтобы завершить работу): La la land
Название: La La Land, Год: 2016, Тип: movie
Название: Going Down in LA-LA Land, Год: 2011, Тип: movie
Название: Fi Al La La Land, Год: 2017, Тип: series
Название: La La Land, Год: 2010, Тип: series
Название: Gary Numan: Android in La La Land, Год: 2016, Тип: movie
Название: La La Land, Год: 2009, Тип: movie
Название: Living in LA LA Land, Год: 2011–2013, Тип: series
Название: La La Land, Год: 2012–, Тип: series
Название: LA LA Land, Год: 2008, Тип: movie
Название: Demi Lovato: La La Land, Год: 2008, Тип: movie
Введите название фильма (или напишите 'выход' чтобы завершить работу): Выход
Завершение работы...
```

## Подсказки

### Работа с API и HTTP-запросами

```python
import requests

response = requests.get("https://api.agify.io?name=michael")
if response.status_code == 200:
    print(response.json())  # {'name': 'michael', 'age': 40, 'count': 12345}
```

### Обработка JSON-ответов

```python
import json

json_data = '{"name": "Alice", "age": 25}'
data = json.loads(json_data)  # Преобразование строки JSON в словарь
print(data["name"])  # Alice
```

### Бесконечный цикл для CLI-программы

```python
while True:
    command = input("Enter command (or 'exit' to quit): ")
    if command.lower() == "exit":
        print("Goodbye!")
        break
    print(f"You entered: {command}")
```

### Формирование URL с параметрами

```python
base_url = "https://api.example.com/search"
query = "python"
full_url = f"{base_url}?q={query}"
print(full_url)  # https://api.example.com/search?q=python
```

### Обработка ошибок HTTP-запросов

```python
response = requests.get("https://api.example.com/data")
if response.status_code == 404:
    print("Error: Not Found")
elif response.status_code == 200:
    print("Success:", response.json())
```

### Форматированный вывод в консоли

```python
movies = [
    {"Title": "Inception", "Year": "2010"},
    {"Title": "The Matrix", "Year": "1999"},
]

for movie in movies:
    print(f"🎬 {movie['Title']} ({movie['Year']})")
```
import requests
import json

API_KEY = "505480d7"
BASE_URL = "http://www.omdbapi.com/"

while True:
    command = input("Enter movie name (or 'exit' to quit): ")
    if command.lower() == "exit":
        print("Goodbye!")
        break
    params = {
        "apikey": API_KEY,
        "s": command
    }
    try:
        response = requests.get(BASE_URL, params=params)
    except requests.RequestException:
        print("Error: Could not connect to OMDb API.")
        continue

    if response.status_code != 200:
        print(f"Error: Server returned status code {response.status_code}")
        continue

    data = response.json()
    if data.get("Response") != "True":
        print("No movies found.")
        continue

    for movie in data.get("Search", []):
        title = movie.get("Title", "Unknown")
        year = movie.get("Year", "Unknown")
        mtype = movie.get("Type", "Unknown")
        print(f"Название: {title}, Год: {year}, Тип: {mtype}")

