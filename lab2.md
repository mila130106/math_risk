import math

options = {
    'Акції': {
        'успіх': {'ймовірність': 0.6, 'прибуток': 500},
        'невдача': {'ймовірність': 0.4, 'прибуток': 300}
    },
    'Облігації': {
        'успіх': {'ймовірність': 0.9, 'прибуток': 440},
        'невдача': {'ймовірність': 0.1, 'прибуток': 120}
    }
}

def expected_value(option):
    return (
        option['успіх']['ймовірність'] * option['успіх']['прибуток'] +
        option['невдача']['ймовірність'] * option['невдача']['прибуток']
    )

def variance(option):
    mean = expected_value(option)
    return (
        option['успіх']['ймовірність'] * (option['успіх']['прибуток'] - mean) ** 2 +
        option['невдача']['ймовірність'] * (option['невдача']['прибуток'] - mean) ** 2
    )

results = {}

for name, data in options.items():
    ev = expected_value(data)
    var = variance(data)
    std_dev = math.sqrt(var)
    results[name] = {'EV': ev, 'Variance': var, 'StdDev': std_dev}

    print(f"{name}:")
    print(f"  Очікуваний прибуток = {ev:.2f} тис. дол.")
    print(f"  Дисперсія (ризик) = {var:.2f}")
    print(f"  Середньоквадратичне відхилення = {std_dev:.2f}\n")

less_risky = min(results, key=lambda x: results[x]['Variance'])
print(f"Менш ризиковане рішення: {less_risky}")

<img width="398" height="278" alt="image" src="https://github.com/user-attachments/assets/a5cd533d-c95a-477b-bc58-97d8f01baf63" />

