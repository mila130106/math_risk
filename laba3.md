

initial_capital = 150.0   
alpha = 0.15           

r_a = 0.05
final_a = initial_capital * (1 + r_a)
U_a = alpha * final_a**2
expected_wealth_a = final_a
certainty_equivalent_a = final_a
risk_premium_a = expected_wealth_a - certainty_equivalent_a

r_success = 0.20
p_success = 0.7
p_fail = 1 - p_success
final_success = initial_capital * (1 + r_success)
final_fail = initial_capital  

U_success = alpha * final_success**2
U_fail = alpha * final_fail**2
EU_b = p_success * U_success + p_fail * U_fail

expected_wealth_b = p_success * final_success + p_fail * final_fail
certainty_equivalent_b = (EU_b / alpha) ** 0.5
risk_premium_b = expected_wealth_b - certainty_equivalent_b

better_choice = "a (державні цінні папери)" if U_a > EU_b else "b (акції)"

print("=== Результати обчислень (тис. $) ===")
print(f"Початковий капітал: {initial_capital}")

print("\n--- Варіант A: Державні папери ---")
print(f"Кінцевий капітал: {final_a:.2f}")
print(f"Корисність: {U_a:.2f}")
print(f"Еквівалент певності: {certainty_equivalent_a:.2f}")
print(f"Премія за ризик: {risk_premium_a:.2f}")

print("\n--- Варіант B: Акції ---")
print(f"Кінцевий капітал (успіх): {final_success:.2f}")
print(f"Кінцевий капітал (невдача): {final_fail:.2f}")
print(f"Очікуваний капітал: {expected_wealth_b:.2f}")
print(f"Очікувана корисність: {EU_b:.2f}")
print(f"Еквівалент певності: {certainty_equivalent_b:.2f}")
print(f"Премія за ризик: {risk_premium_b:.2f}")

print("\nВисновок: Краще обрати", better_choice)
<img width="427" height="532" alt="image" src="https://github.com/user-attachments/assets/947c5b6d-77fe-49de-b727-b20d8d5bd4b8" />
