# Game_strategy
//загружаем все используемые библиотеки
#include <iostream>
#include <unistd.h>
#include <cstdlib>
#include <ctime>
using namespace std;
//создаем класс, план для будущих объектов
class enemies {
  private:
    int health, damage;
    string name;
  public:  
    enemies(int x = 100, int y = 25, string z = "No name") {
      HP(x);
      DMG(y);
      PCT(z);
    }
    int HP(int x = -100) {
      if (x == -100) {
        return health;
      }
      else {
        health = x;
      }
    }
    int DMG(int x = -100) {
      if (x == -100) {
        return damage;
      }
      else {
        damage = x;
      }
    }
    string PCT(string x = "Nothing") {
      if (x == "Nothing") {
        return name;
      }
      else {
        name = x;
      }
    }
};
//функция рандомайзер, высчитывает в диапазоне чисел, заданных через аргумент
int randVar(int x) {
  srand(time(0) + !x);
  return 1 + (rand() % x);
}

int main() {
  //создаём объекты, и определяем для них параметры
  enemies Wolf(50, 25, "Серый волк");//здоровье; урон; название соперника
  enemies Bear(85, 20, "Дикий медведь");
  enemies Dragon(100, 30, "Огнедышащий дракон");
  int healthUser = 100;
  auto enemy = Wolf;//инициализируем переменную, через любой объекта класса "enemies"
  //определяем объект для дальнейшей работы с ним с помощью рандомайзера
  switch (randVar(89)) {
    case 1:
      enemy = Wolf;
      break;
    case 2:
      enemy = Bear;
      break;
    case 3:
      enemy = Dragon;
      break;
  } 
  cout << "        Враг объявлен - " << enemy.PCT() << endl;
  cout << "      Здоровье противника составляет: " << enemy.HP() << "HP" << endl;
  bool move = true;
    
  while (bool process = true) {
    if (healthUser < 1) {
      sleep(3);
      cout << "\n                Вы проиграли." << endl;
      return process = false;
    }
    else {
      if (enemy.HP() < 1) {
        sleep(3);
        cout << "\n             Поздравляю с победой!" << endl;
        return process = false;
      }
    }    
    if (move) {
      int variousPerson;
      sleep(3);
      cout << "\n                    Ваш ход:" << endl;
      sleep(2);
      cout << "\n1 - обычный удар (-15HP) 🔪" << endl;
      cout << "2 - специальный удар (-30HP / промахнуться) 🏹" << endl;
      cout << "3 - излечение (+25HP) ➕" << endl;
      cout << "\nВведите один из вариантов: ";
      cin >> variousPerson;
      
      if (variousPerson == 1) {
        enemy.HP(enemy.HP() - 15);
        if (enemy.HP() < 0) {
          enemy.HP(0);
        }
        cout << "\nВы нанесли урон в размере 15HP" << endl;
        cout << "Здоровье противника теперь составляет: " << enemy.HP() << "HP" << endl;
        if (enemy.HP() > 0) {
          cout << "... Ход передается противнику." << endl;
          move = false;
        }
      }
      if (variousPerson == 2) {   
        if (randVar(2) == 1) {
          enemy.HP(enemy.HP() - 30);
          if (enemy.HP() < 0) {
            enemy.HP(0);
          }
          cout << "\nВам повезло, и вы нанесли урон в размере 30HP" << endl;
          cout << "Здоровье противника теперь составляет: " << enemy.HP() << "HP" << endl;
          if (enemy.HP() > 0) {
            cout << "... Ход передается противнику." << endl;
            move = false;
          }
        }
        else {
          cout << "\nК сожалению вы промахнулись" << endl;
          cout << "Здоровье противника, пока что составляет: " << enemy.HP() << "HP"<< endl;
          cout << "... Ход передается противнику." << endl;
          move = false;
        }
      }
      if (variousPerson == 3) {
        if (healthUser != 100) {
          healthUser = healthUser + 25;
          if (healthUser > 100) {
            healthUser = healthUser - 35;
            cout << "\nПроизошла передозировка лекараства" << endl;
            cout << "Ваше здоровье составляет: " << healthUser << "HP" << endl;
            cout << "... Ход передается противнику." << endl;
            move = false;
          }
          else {
            cout << "\nВы успешно восстановили здоровье" << endl;
            cout << "Теперь оно составляет: " << healthUser << "HP" << endl;
            cout << "... Ход передается противнику." << endl;
            move = false;
          }
        }
        else {
          cout << "\nВаше здоровье, и так составляет 100HP" << endl;
          cout << "Перевыберите действие." << endl;
        }
      }
    }
    else {
      int variousEnemy = randVar(3);
      sleep(3);
      cout << "\n                Ход противника:" << endl;
      sleep(2);
      if (variousEnemy == 1) {
        healthUser = healthUser - enemy.DMG();
        if (healthUser < 0) {
          healthUser = 0;
        }
        cout << "\nПротивник нанес урон в размере " << enemy.DMG() << "HP" << endl;
        cout << "Ваше здоровье теперь составляет: " << healthUser << "HP" << endl;
        if (healthUser > 0) {
          cout << "... Следующий ход за вами." << endl;
          move = true;
        }
      }
      if (variousEnemy == 2) {
        if (randVar(2) == 1) {
          healthUser = healthUser - (enemy.DMG() * 2);
          if (healthUser < 0) {
            healthUser = 0;
          }
          cout << "\nПротивнику повезло, и он нанес урон в размере " << (enemy.DMG() * 2) << "HP" << endl;
          cout << "Ваше здоровье теперь составляет: " << healthUser << "HP" << endl;
          if (healthUser > 0) {
            cout << "... Следующий ход за вами." << endl;
            move = true;
          }
        }
        else {
          cout << "\nВ ходе битвы противник промахнулся" << endl;
          cout << "Ваше здоровье, пока что составляет: " << healthUser << "HP"<< endl;
          cout << "... Следующий ход за вами." << endl;
          move = true;
        }
      }
      if (variousEnemy == 3) {
        if (enemy.HP() < 100) {
          enemy.HP(enemy.HP() + 25);
          if (enemy.HP() > 100) {
            enemy.HP(100);
          }
          cout << "\nПротивник успешно восстановил здоровье" << endl;
          cout << "Теперь оно составляет: " << enemy.HP() << "HP" << endl;
          cout << "... Следующий ход за вами." << endl;
          move = true;
        }
        else {
          cout << "\nПротивник решил пропустить ход" << endl;
          cout << "... Следующий ход за вами." << endl;
          move = true;
        }
      }
    }
  }
  
  return 0;
}
