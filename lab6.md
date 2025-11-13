import numpy as np
import pandas as pd

F1 = np.array([
    [2.5, 3.5, 4.0],
    [4.5, 2.0, 3.5],
    [3.0, 6.0, 2.5],
    [5.5, 1.5, 3.5]
])

F2 = np.array([
    [20, 35, 50],
    [60, 30, 30],
    [25, 85, 40],
    [80, 10, 35]
])

P = np.array([1/2, 1/3, 1/6])

W = np.array([3, 1])

decisions = ['x1', 'x2', 'x3', 'x4']

M1 = np.dot(F1, P)

M2 = np.dot(F2, P)

print("--- 2. Очікувані значення ---")
for i, d in enumerate(decisions):
    print(f"Рішення {d}: M1 (збитки) = {M1[i]:.3f}, M2 (продукція) = {M2[i]:.3f}")
print("-" * 35)

min_M1, max_M1 = M1.min(), M1.max()
M1_hat = (max_M1 - M1) / (max_M1 - min_M1)

min_M2, max_M2 = M2.min(), M2.max()
M2_hat = (M2 - min_M2) / (max_M2 - min_M2)

W_integrated = W[0] * M1_hat + W[1] * M2_hat

optimal_index = np.argmax(W_integrated)
optimal_decision = decisions[optimal_index]
max_W = W_integrated[optimal_index]


results_df = pd.DataFrame({
    'Рішення': decisions,
    'M1 (Збитки)': M1,
    'M2 (Продукція)': M2,
    'M1_hat (Нормал.)': M1_hat.round(4),
    'M2_hat (Нормал.)': M2_hat.round(4),
    'W(x) (Компроміс)': W_integrated.round(4)
})

print("--- 6. Зведена таблиця результатів ---")
print(results_df.to_string(index=False))

print("\n" + "=" * 50)
print(f"Оптимальне рішення за Критерієм Компромісу: {optimal_decision}")
print(f"Максимальне зважене значення W: {max_W:.4f}")
print("=" * 50)
