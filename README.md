# Лабораторна робота №2 (Варіант 9)

**Студент:** Кріль М. Б.[cite: 8]  
**Група:** ІСТ-21/1[cite: 8]  

---

## Завдання 9

Створити клас `РЕЦЕПТ` кулінарного виробу, який містить набір об’єктів класу `ІНГРЕДІЄНТ`. Клас інгредієнтів містить назву продукту, дозування та ціну одиниці продукції. Визначити вартість виготовлення виробу згідно з рецептом. Зберегти рецепт у файл та здійснити пошук рецепта у файлі.

---

## Вихідний код програми (C++)

```cpp
#include <iostream>
#include <string>
#include <vector>
#include <fstream>
#include <iomanip>
#include <windows.h>

using namespace std;

class Ingredient {
    string name; double dos, price;
public:
    Ingredient(string n="", double d=0, double p=0) : name(n), dos(d), price(p) {}
    void set(string n, double d, double p) { name = n; dos = d; price = p; }
    string getName() const { return name; }
    double getCost() const { return dos * price; }
    void show() const { cout << "  - " << name << ": " << dos << " x " << price << " = " << getCost() << " UAH\n"; }
    void save(ofstream& f) const { f << name << " " << dos << " " << price << "\n"; }
};

class Recipe {
    string rName; vector<Ingredient> ings;
public:
    Recipe(string n="") : rName(n) {}
    void set(string n) { rName = n; }
    void add(const Ingredient& i) { ings.push_back(i); }
    double getTotal() const { double t = 0; for(auto& i : ings) t += i.getCost(); return t; }
    void show() const {
        cout << "\nRECIPE: " << rName << "\n------------------\n";
        for(auto& i : ings) i.show();
        cout << "Total: " << getTotal() << " UAH\n";
    }
    void saveToFile(const string& file) const {
        ofstream f(file, ios::app);
        f << "RECIPE " << rName << " " << ings.size() << "\n";
        for(auto& i : ings) i.save(f);
    }
    static void findInFile(const string& file, const string& target) {
        ifstream f(file); string tag, name; int cnt;
        while(f >> tag >> name >> cnt) {
            Recipe r(name);
            for(int i = 0; i < cnt; ++i) {
                string in; double d, p; f >> in >> d >> p;
                r.add(Ingredient(in, d, p));
            }
            if(name == target) { cout << "\n[FOUND]"; r.show(); return; }
        }
        cout << "\nNot found: " << target << "\n";
    }
};

int main() {
    SetConsoleOutputCP(65001);
    SetConsoleCP(65001);

    cout << fixed << setprecision(2);
    string file = "rec.txt";

    Recipe r1("Pie");
    r1.add(Ingredient("Flour", 0.5, 24));
    r1.add(Ingredient("Sugar", 0.2, 30));
    r1.add(Ingredient("Eggs", 4, 4.5));

    r1.show();
    r1.saveToFile(file);

    Recipe::findInFile(file, "Pie");
    return 0;
}
```
РЕЗУЛЬТАТИ ВИКОНАННЯ ПРОГРАМИ 
```
RECIPE: Pie
------------------
  - Flour: 0.50 x 24.00 = 12.00 UAH
  - Sugar: 0.20 x 30.00 = 6.00 UAH
  - Eggs: 4.00 x 4.50 = 18.00 UAH
Total: 36.00 UAH

[FOUND]
RECIPE: Pie
------------------
  - Flour: 0.50 x 24.00 = 12.00 UAH
  - Sugar: 0.20 x 30.00 = 6.00 UAH
  - Eggs: 4.00 x 4.50 = 18.00 UAH
Total: 36.00 UAH
```
ВИСНОВОК 
Під час виконання лабораторної роботи було успішно освоєно принципи об'єктно-орієнтованого програмування, зокрема роботу з класами та масивами об'єктів у C++.
На практиці реалізовано взаємодію між класами Ingredient та Recipe, виконано розрахунок загальної вартості виробу, а також налаштовано збереження даних у файл і подальший пошук необхідної інформації.
