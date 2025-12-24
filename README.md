import csv
from io import StringIO

csv_text = """Country Name,Indicator Name,2018,2019
Ukraine,Inflation,10.9,7.9
Germany,Inflation,1.9,1.4
Poland,Inflation,2.5,3.4
France,Inflation,1.8,1.6
"""

# імітація файлу
file = StringIO(csv_text)

reader = csv.DictReader(file)
data = list(reader)

print("Вміст CSV файлу:\n")
for row in data:
    print(row)

countries = input("\nВведіть назви країн через кому: ").split(",")
countries = [c.strip() for c in countries]

result = []
for row in data:
    if row["Country Name"] in countries:
        result.append({
            "Country Name": row["Country Name"],
            "2018": row["2018"],
            "2019": row["2019"]
        })



Завдання 2 


import json
import os

DATA_FILE = "countries.json"
RESULT_FILE = "result.json"

def load_data():
    if not os.path.exists(DATA_FILE):
        return []
    with open(DATA_FILE, "r", encoding="utf-8") as file:
        return json.load(file)

def save_data(data):
    with open(DATA_FILE, "w", encoding="utf-8") as file:
        json.dump(data, file, ensure_ascii=False, indent=4)

def show_data():
    data = load_data()
    if not data:
        print("Файл порожній.")
        return
    for country in data:
        print(country)

def add_country():
    name = input("Назва держави: ")
    population = float(input("Населення (млн): "))
    area = float(input("Площа (тис. км²): "))

    data = load_data()
    data.append({
        "name": name,
        "population": population,
        "area": area
    })
    save_data(data)
    print("Держава додана.")

def delete_country():
    name = input("Введіть назву держави для видалення: ")
    data = load_data()
    new_data = [c for c in data if c["name"].lower() != name.lower()]

    if len(data) == len(new_data):
        print("Державу не знайдено.")
    else:
        save_data(new_data)
        print("Державу видалено.")

def search_country():
    name = input("Введіть назву держави для пошуку: ")
    data = load_data()
    for country in data:
        if country["name"].lower() == name.lower():
            print(country)
            return
    print("Державу не знайдено.")

def max_density():
    data = load_data()
    if not data:
        print("Немає даних.")
        return

    max_country = None
    max_density_value = 0

    for country in data:
        density = country["population"] / country["area"]
        if density > max_density_value:
            max_density_value = density
            max_country = country

    result = {
        "name": max_country["name"],
        "density": round(max_density_value, 2)
    }

    with open(RESULT_FILE, "w", encoding="utf-8") as file:
        json.dump(result, file, ensure_ascii=False, indent=4)

    print("Результат записано у result.json")
    print(result)
initial_data = [
    {"name": "Україна", "population": 41, "area": 603.7},
    {"name": "Польща", "population": 38, "area": 312.7},
    {"name": "Франція", "population": 67, "area": 551.5}
]

with open("countries.json", "w", encoding="utf-8") as file:
    json.dump(initial_data, file, ensure_ascii=False, indent=4)



def menu():
    while True:
        print("\n--- МЕНЮ ---")
        print("1 - Показати всі дані")
        print("2 - Додати державу")
        print("3 - Видалити державу")
        print("4 - Пошук за назвою")
        print("5 - Держава з максимальною щільністю населення")
        print("0 - Вихід")

        choice = input("Ваш вибір: ")

        if choice == "1":
            show_data()
        elif choice == "2":
            add_country()
        elif choice == "3":
            delete_country()
        elif choice == "4":
            search_country()
        elif choice == "5":
            max_density()
        elif choice == "0":
            break
        else:
            print("Невірний вибір.")

menu()

print("\nРезультат пошуку:\n")
if result:
    for r in result:
        print(r)
else:
    print("Нічого не знайдено")
