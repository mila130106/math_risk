def best_choice():
    p = float(input("Введіть ймовірність дощу (від 0 до 1): "))
    
    r_hd = int(input("Оцініть (1-10): сидіти вдома, коли дощ: "))
    r_hs = int(input("Оцініть (1-10): сидіти вдома, коли сонце: "))
    r_os = int(input("Оцініть (1-10): вийти на вулицю, коли сонце: "))
    r_od = int(input("Оцініть (1-10): вийти на вулицю, коли дощ: "))

    U_home = p * r_hd + (1 - p) * r_hs
    U_out = p * r_od + (1 - p) * r_os

    print("\nОчікувані оцінки:")
    print(f"Сидіти вдома: {U_home:.2f}")
    print(f"Вийти на вулицю: {U_out:.2f}")

    if U_home > U_out:
        print("\n👉 Найкращий вибір: сидіти вдома")
    elif U_out > U_home:
        print("\n👉 Найкращий вибір: вийти на вулицю")
    else:
        print("\n🤝 Обидва варіанти однаково хороші")


best_choice()

<img width="496" height="291" alt="image" src="https://github.com/user-attachments/assets/35f545ee-4132-4056-85f3-9abd7d16c34b" />

