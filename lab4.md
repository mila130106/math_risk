import numpy as np
import pandas as pd

# Період: 1, 2, 3, 4, 5
# A1: 15, 13, 12, 13, 17
# A2: 16, 20, 12, 20, 12
returns = {
    'A1': [15, 13, 12, 13, 17],
    'A2': [16, 20, 12, 20, 12]
}
df = pd.DataFrame(returns)


m1 = df['A1'].mean()
m2 = df['A2'].mean()

sigma1_sq = df['A1'].var(ddof=1)
sigma2_sq = df['A2'].var(ddof=1)
sigma1 = np.sqrt(sigma1_sq)
sigma2 = np.sqrt(sigma2_sq)

rho12 = df['A1'].corr(df['A2'])

print("--- Обчислені вихідні параметри ---")
print(f"Сподівана норма прибутку A1 (m1): {m1:.2f}%")
print(f"Сподівана норма прибутку A2 (m2): {m2:.2f}%")
print(f"Оцінка ризику A1 (sigma1): {sigma1:.2f}% (sqrt({sigma1_sq:.2f}))")
print(f"Оцінка ризику A2 (sigma2): {sigma2:.2f}% (sqrt({sigma2_sq:.2f}))")
print(f"Коефіцієнт кореляції (rho12): {rho12:.4f}\n")

def calc_m(x1, m1, m2):
    """Обчислення очікуваної норми прибутку портфеля m_P"""
    return x1 * m1 + (1 - x1) * m2

def calc_sigma(x1, sigma1, sigma2, rho12):
    """Обчислення ризику портфеля sigma_P"""
    # Використовуємо формулу, що містить rho12
    sigma_p_sq = (x1**2 * sigma1**2 + 
                  (1 - x1)**2 * sigma2**2 + 
                  2 * x1 * (1 - x1) * rho12 * sigma1 * sigma2)
    return np.sqrt(sigma_p_sq)

results = []



x1_min_risk = (sigma2_sq - rho12 * sigma1 * sigma2) / \
              (sigma1_sq + sigma2_sq - 2 * rho12 * sigma1 * sigma2)
x2_min_risk = 1 - x1_min_risk

m_min_risk = calc_m(x1_min_risk, m1, m2)
sigma_min_risk = calc_sigma(x1_min_risk, sigma1, sigma2, rho12)

results.append(("Мінімальний ризик (а)", x1_min_risk, m_min_risk, sigma_min_risk))

print("--- а) Портфель мінімального ризику ---")
print(f"Частка A1 (x1): {x1_min_risk:.4f}")
print(f"Частка A2 (x2): {x2_min_risk:.4f}")
print(f"Сподівана норма прибутку (m_P): {m_min_risk:.2f}%")
print(f"Ризик (sigma_P): {sigma_min_risk:.2f}%\n")

R_F = 10.0

numerator_T = (m1 - R_F) * sigma2_sq - (m2 - R_F) * (rho12 * sigma1 * sigma2) # rho12 * sigma1 * sigma2 = sigma12
denominator_T = (m1 - R_F) * sigma2_sq + (m2 - R_F) * sigma1_sq - (m1 - R_F + m2 - R_F) * (rho12 * sigma1 * sigma2)

if denominator_T != 0:
    x1_tangent = numerator_T / denominator_T
    x2_tangent = 1 - x1_tangent
    m_tangent = calc_m(x1_tangent, m1, m2)
    sigma_tangent = calc_sigma(x1_tangent, sigma1, sigma2, rho12)
    
    results.append(("Танґенційний (ринковий) (б)", x1_tangent, m_tangent, sigma_tangent))

    print("--- б) Танґенційний портфель (Ринковий, Rf=10%) ---")
    print(f"Частка A1 (x1): {x1_tangent:.4f}")
    print(f"Частка A2 (x2): {x2_tangent:.4f}")
    print(f"Сподівана норма прибутку (m_P): {m_tangent:.2f}%")
    print(f"Ризик (sigma_P): {sigma_tangent:.2f}%\n")
else:
    print("--- б) Танґенційний портфель (Ринковий, Rf=10%) ---")
    print("Знаменник дорівнює нулю. Танґенційний портфель не визначений.\n")

R_target_1 = 15.25

x1_target_1 = (R_target_1 - m2) / (m1 - m2)
x2_target_1 = 1 - x1_target_1
sigma_target_1 = calc_sigma(x1_target_1, sigma1, sigma2, rho12)

results.append((f"Цільовий (в) m_P={R_target_1}%", x1_target_1, R_target_1, sigma_target_1))

print(f"--- в) Цільовий портфель (m_P = {R_target_1}%) ---")
print(f"Частка A1 (x1): {x1_target_1:.4f}")
print(f"Частка A2 (x2): {x2_target_1:.4f}")
print(f"Сподівана норма прибутку (m_P): {R_target_1:.2f}% (за умовою)")
print(f"Ризик (sigma_P): {sigma_target_1:.2f}%\n")



R_target_2 = 3.5

x1_target_2 = (R_target_2 - m2) / (m1 - m2)
x2_target_2 = 1 - x1_target_2
sigma_target_2 = calc_sigma(x1_target_2, sigma1, sigma2, rho12)

results.append((f"Цільовий (г) m_P={R_target_2}%", x1_target_2, R_target_2, sigma_target_2))

print(f"--- г) Цільовий портфель (m_P = {R_target_2}%) ---")
print(f"Частка A1 (x1): {x1_target_2:.4f}")
print(f"Частка A2 (x2): {x2_target_2:.4f}")
print(f"Сподівана норма прибутку (m_P): {R_target_2:.2f}% (за умовою)")
print(f"Ризик (sigma_P): {sigma_target_2:.2f}%\n")


results_df = pd.DataFrame(results, columns=["Портфель", "x1 (A1)", "m_P (%)", "sigma_P (%)"])
results_df['x2 (A2)'] = 1 - results_df['x1 (A1)']
results_df = results_df[['Портфель', 'x1 (A1)', 'x2 (A2)', 'm_P (%)', 'sigma_P (%)']]

results_df['x1 (A1)'] = results_df['x1 (A1)'].round(4)
results_df['x2 (A2)'] = results_df['x2 (A2)'].round(4)
results_df['m_P (%)'] = results_df['m_P (%)'].round(2)
results_df['sigma_P (%)'] = results_df['sigma_P (%)'].round(2)


print("--- д) Зведена таблиця результатів ---")
print(results_df.to_string(index=False))
