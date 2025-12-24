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

print("\nРезультат пошуку:\n")
if result:
    for r in result:
        print(r)
else:
    print("Нічого не знайдено")
