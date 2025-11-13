import pandas as pd
import numpy as np

profit_matrix = np.array([
    [6.0, 3.5, 0.5],  # x1: Засоби догляду
    [6.5, 7.0, 4.0],  # x2: Дрібні запчастини
    [3.5, 3.5, 8.5]   # x3: Газетно-журнальна продукція
])

probabilities = np.array([
    0.2,  # P(theta1)
    0.4,  # P(theta2)
    0.4   # P(theta3)
])

decisions = ['x1 (Догляд)', 'x2 (Запчастини)', 'x3 (Преса)']

expected_profits = []
for i, row in enumerate(profit_matrix):
    expected_profit = np.sum(row * probabilities)
    expected_profits.append(expected_profit)
    print(f"Очікуваний прибуток для {decisions[i]}: {expected_profit:.2f}")

max_profit = max(expected_profits)
optimal_index = expected_profits.index(max_profit)
optimal_decision = decisions[optimal_index]

print("\n-------------------------------------------")
print(f"Максимальний очікуваний прибуток: {max_profit:.2f}")
print(f"Оптимальне рішення (x*): {optimal_decision}")
print("-------------------------------------------")

results = pd.DataFrame({
    'Рішення': decisions,
    'Очікуваний прибуток (M(xi))': expected_profits
})

results = results.sort_values(by='Очікуваний прибуток (M(xi))', ascending=False)
results['Оптимальність'] = np.where(results['Рішення'] == optimal_decision, 'Оптимальне', '')

print("\nЗведена таблиця результатів:")
print(results.to_string(index=False))
