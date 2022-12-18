# Calculation-of-the-linear-navigation-gate
This project calculate valums for gates navigating for GUMRF cadets.
print("Введите первую цифру варианта: ")
nomer1 = input()
print("Введите вторую цифру варианта: ")
nomer2 = input()
# База данных 1 из приложения 2, последняя цифра варианта
Base1 = [
    [1.0, 8.4, 160, 0.84, "Тёмный", 9.5, 11.5],
    [3.0, 5.0, 150, 0.7, "Светлый", 5.0, 7.0],
    [2.8, 5.4, 160, 0.84, "Тёмный", 5.5, 7.5],
    [2.6, 5.8, 180, 0.7, "Светлый", 6.0, 8.0],
    [2.4, 6.2, 180, 0.84, "Тёмный", 6.5, 8.5],
    [2.2, 6.5, 190, 0.7, "Светлый", 7.0, 9.0],
    [2.0, 7.0, 200, 0.84, "Тёмный", 7.5, 9.5],
    [1.9, 7.4, 190, 0.84, "Светлый", 8.0, 10.0],
    [1.6, 7.7, 180, 0.84, "Тёмный", 8.5, 10.5],
    [1.3, 8.1, 170, 0.84, "Светлый", 9.0, 11.0]
]
# База данных 2 из приложения 1, первая цифра варианта
Base2 = [
    ["толк сост", 16.5, 270, 16.7, 220, 8.5],
    ["толк сост", 16.0, 290, 14.0, 230, 8.0],
    ["сухогр", 19.0, 110, 16.7, 90, 7.0],
    ["пасс", 25.0, 105, 16.8, 58, 8.0],
    ["метеор", 68, 34, 6.0, 15, 6.0],
    ["ракета", 60, 27, 5.0, 18, 5.0],
    ["ракета", 61, 27, 5.0, 18, 5.0],
    ["метеор", 67, 34, 6.0, 15, 6.0],
    ["пасс", 23.0, 120, 16.0, 60, 9.0],
    ["сухогр", 18.5, 130, 13.0, 105, 8.0]
]
D0 = Base1[int(nomer2)][0]
Dk = Base1[int(nomer2)][1]
B = Base1[int(nomer2)][2]
Tatm = Base1[int(nomer2)][3]
Fon = Base1[int(nomer2)][4]
h2 = Base1[int(nomer2)][5]
H2 = Base1[int(nomer2)][6]
Tip = Base2[int(nomer1)][0]
V = Base2[int(nomer1)][1]
L = Base2[int(nomer1)][2]
q = Base2[int(nomer1)][3]
l = Base2[int(nomer1)][4]
e = Base2[int(nomer1)][5]
print("Дано: ", "D0 = " + str(D0), "Dk = " + str(Dk), "B = " + str(B), "Tatm = " + str(Tatm),
      "Fon = " + str(Fon), "h2 = " + str(h2), "H2 = " + str(H2), "Tip = " + str(Tip), "V = " + str(V),
      "L = " + str(L), "q = " + str(q), "l = " + str(l), "e = " + str(e), sep="\n")
if Tip == "пасс" or Tip == "сухогр":
    t = 20
elif Tip == "толк сост":
    t = 50
else:
    t = 14
if t == 14:
    R = 800
else:
    R = 7 * L
Pop = 0.035*(V / 36 * 10 * t + l) + 0.57 * q + 0.197 * L * L / R
print("Роп = ", Pop)
Pdop = (B / 2) - Pop
print("Рдоп = ", Pdop)
d = Dk * Dk / (2.29 * Pdop - Dk)
print("d = ", d)
# базы данных для таблицы щитов
Base3 = [ #0.7
    [60, 1.0],
    [90, 1.5],
    [120, 2.0],
    [180, 3.0],
    [320, 3.0],
    [380, 3.5],
    [500, 4.0],
    [600, 5.0],
    [600, 6.0],
    [600, 6.0],
    [800, 6.5]
]
Base4 = [ #0.84
    [60, 1.5],
    [90, 2.0],
    [120, 2.5],
    [180, 3.5],
    [320, 4.0],
    [380, 4.5],
    [500, 5.5],
    [600, 7.0],
    [600, 8.0],
    [600, 8.5],
    [800, 10.0]
]
peremen = 0
h1 = 0
if Tatm == 0.7:
    while Dk > Base3[int(peremen)][1]:
        peremen += 1
        h1 = Base3[int(peremen)][0]
else:
    while Dk > Base4[int(peremen)][1]:
        peremen += 1
        h1 = Base4[int(peremen)][0]
h1 = h1 / 100
h = int(h1) + h2 + 1
print("h = ", h)
H0 = (D0 + d) * ((h - e) / D0 + 0.063 * d + 0.87) + e
Hk = (Dk + d) * ((h - e) / Dk + 0.063 * d + 0.87) + e
H = max(H0, Hk)
print("H = ", H)
H1 = H - H2
print("H1 = ", H1)
Damax = pow(h - e, 0.5) / (pow(H - e, 0.5) - pow(h - e, 0.5)) * d
print("Damax = ", Damax)
a = 0
Dper = D0
while Dper < Dk:
    a = 3.44 * ((H - e) / (Dper + d) - (h - e) / Dper - 0.063 * d)
    print("Для а по D = ", Dper, "a = ", a)
    Dper += 1
a = 3.44 * ((H - e) / (Dk + d) - (h - e) / Dk - 0.063 * d)
print("Для а по Dk ", "a = ", a)
Pper = 0
Dper = D0
while Dper < Dk:
    Pper = 0.29 * 1.5 * Dper * (Dper / d + 1)
    print("Для P по D = ", Dper, "P = ", Pper)
    Dper += 1
Pper = 0.29 * 1.5 * Dk * (Dk / d + 1)
print("Для P по Dk ", "P = ", Pper)
Dkfin = d / 2 * (pow(1 + 13.8 * Pdop / d / 1.5, 0.5) - 1)
print("Dk = ", Dkfin)
print("Конец")
