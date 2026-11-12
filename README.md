# Контрольная работа №2. 

**Вариант:** 7 - Водитель



**Условие задания:**
Задание: Придумайте иерархию классов. 
(10 баллов максимально)
Требования к реализации:
Иерархия должна состоять минимум из трех классов.
(2 балла)

Базовый родительский класс должен быть абстрактным.
(1 балл)
В родительском классе необходимо определить минимум два полиморфных метода (не считая деструктора) и переопределить их в каждом классе-потомке.
(1 балл)
Создать один объект с помощью механизма позднего связывания, тип объекта должен выбирать пользователь.
(2 балл)
С помощью раннего связывания создать по одному объекту для каждого не абстрактного класса.
(2 балл)
Добавить static поле для подсчета созданных объектов. В программе вывести это количество. 


**Код программы:**
```cpp
#include <iostream>
#include <string>
#include <clocale>
using namespace std;

class Driver {
protected:
    string fullName;
    int experience;
    static int count;

public:
    Driver(string n, int e) : fullName(n), experience(e) {
        count++;
    }

    virtual ~Driver() {}

    virtual void showInfo() const = 0;
    virtual string getCategory() const = 0;

    int getExperience() const { return experience; }

    static int getCount() { return count; }
};

int Driver::count = 0;

class TruckDriver : public Driver {
private:
    double tonnage;

public:
    TruckDriver(string n, int e, double t) : Driver(n, e), tonnage(t) {}

    void showInfo() const override {
        cout << "Водитель грузовика: " << fullName << ", стаж: " << experience
            << " лет, грузоподъёмность: " << tonnage << " т" << endl;
    }

    string getCategory() const override {
        return "Категория C (грузовой)";
    }
};

class BusDriver : public Driver {
private:
    int passengers;

public:
    BusDriver(string n, int e, int p) : Driver(n, e), passengers(p) {}

    void showInfo() const override {
        cout << "Водитель автобуса: " << fullName << ", стаж: " << experience
            << " лет, мест: " << passengers << endl;
    }

    string getCategory() const override {
        return "Категория D (пассажирский)";
    }
};

int main() {
    setlocale(LC_ALL, "Russian");
    TruckDriver truckMan("Иванов Иван", 12, 20.5);
    BusDriver busMan("Петров Пётр", 7, 45);

    cout << "Раннее связывание" << endl;
    truckMan.showInfo();
    cout << "Категория: " << truckMan.getCategory() << endl;
    cout << "Стаж: " << truckMan.getExperience() << " лет" << endl;

    busMan.showInfo();
    cout << "Категория: " << busMan.getCategory() << endl;
    cout << "Стаж: " << busMan.getExperience() << " лет" << endl;

    cout << "\nПозднее связывание" << endl;
    int choice;
    do {
        cout << "Выберите водителя (1 - грузовик, 2 - автобус): ";
        cin >> choice;
    } while (choice != 1 && choice != 2);

    Driver* driver = nullptr;
    if (choice == 1)
        driver = new TruckDriver("Сидоров Сидор", 5, 15.0);
    else
        driver = new BusDriver("Кузнецов Алексей", 9, 60);

    driver->showInfo();
    cout << "Категория: " << driver->getCategory() << endl;
    cout << "Стаж: " << driver->getExperience() << " лет" << endl;

    delete driver;

    cout << "\nВсего создано водителей: " << Driver::getCount() << endl;

    return 0;
}
```

**Реализация и компиляция:**

<img width="1200" height="562" alt="image" src="https://github.com/user-attachments/assets/2967539b-0ac6-418e-b47b-31564168654f" />

<img width="1200" height="562" alt="image" src="https://github.com/user-attachments/assets/c3324676-3d7f-4a4a-aefd-30f145569a4b" />

