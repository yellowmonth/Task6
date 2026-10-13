# Отчёт по практической работе №5


**Студенты:**  Бандурин Дмитрий, Карпенко Артём

**Группа:**  П25-2.1 

**Дисциплина:**  Основы алгоритмизации и программирования 

**Тема практической работы:** Двумерные прямоугольные массивы (матрицы) в C# и генерация игровых локаций

**Вариант:** №14 Лаборатория микробиологии (Чашка Петри)


<picture>
  <img src="https://github.com/yellowmonth/task5/blob/main/assets/screens/1.png">
</picture>

<picture>
  <img src="https://github.com/yellowmonth/task5/blob/main/assets/screens/2.png">
</picture>

<picture>
  <img src="https://github.com/yellowmonth/task5/blob/main/assets/screens/3.png">
</picture>

<picture>
  <img src="https://github.com/yellowmonth/task5/blob/main/assets/screens/4.png">
</picture>

<picture>
  <img src="https://github.com/yellowmonth/task5/blob/main/assets/screens/5.png">
</picture>

<picture>
  <img src="https://github.com/yellowmonth/task5/blob/main/assets/screens/6.png">
</picture>




```csharp
using System;

class Program
{
    static void Main()
    {
        const int rows = 5;
        const int cols = 5;

        // Матрица агара
        char[,] petriDish = new char[rows, cols];

        // Матрица численности бактерий
        int[,] bacteriaCount = new int[rows, cols];

        Random rnd = new Random();

        // 1-2. Заполнение чашки: периметр — стекло, центр — агар
        for (int i = 0; i < petriDish.GetLength(0); i++)
        {
            for (int j = 0; j < petriDish.GetLength(1); j++)
            {
                if (i == 0 || i == petriDish.GetLength(0) - 1 ||
                    j == 0 || j == petriDish.GetLength(1) - 1)
                    petriDish[i, j] = '#'; // защитное стекло
                else
                    petriDish[i, j] = '.'; // чистый агар
            }
        }

        // 3. Заполнение численности бактерий от 100 до 5000
        for (int i = 0; i < bacteriaCount.GetLength(0); i++)
        {
            for (int j = 0; j < bacteriaCount.GetLength(1); j++)
            {
                bacteriaCount[i, j] = rnd.Next(100, 5001);
            }
        }

        // 4. Засев 3 колоний бактерий во внутренние клетки
        int posadil = 0;
        while (posadil < 3)
        {
            int i = rnd.Next(1, petriDish.GetLength(0) - 1);
            int j = rnd.Next(1, petriDish.GetLength(1) - 1);

            if (petriDish[i, j] == '.')
            {
                petriDish[i, j] = 'B'; // колония
                posadil++;
            }
        }

        // 5. Вывод планшета с разделением колонок двоеточием
        Console.WriteLine("Планшет микробиолога:");
        for (int i = 0; i < petriDish.GetLength(0); i++)
        {
            for (int j = 0; j < petriDish.GetLength(1); j++)
            {
                Console.Write(petriDish[i, j]);
                if (j < petriDish.GetLength(1) - 1)
                    Console.Write(":");
            }
            Console.WriteLine();
        }

        // 6. Поиск колонии с рекордной численностью (среди 'B')
        int max = -1;
        int maxI = -1;
        int maxJ = -1;

        for (int i = 0; i < bacteriaCount.GetLength(0); i++)
        {
            for (int j = 0; j < bacteriaCount.GetLength(1); j++)
            {
                if (petriDish[i, j] == 'B' && bacteriaCount[i, j] > max)
                {
                    max = bacteriaCount[i, j];
                    maxI = i;
                    maxJ = j;
                }
            }
        }

        Console.WriteLine();
        Console.WriteLine($"Рекордная колония: строка {maxI}, столбец {maxJ}");
        Console.WriteLine($"Численность: {max}");

        // 7. Среднее число бактерий на всём планшете
        int summa = 0;
        for (int i = 0; i < bacteriaCount.GetLength(0); i++)
        {
            for (int j = 0; j < bacteriaCount.GetLength(1); j++)
            {
                summa += bacteriaCount[i, j];
            }
        }
        double srednee = (double)summa /
                         (bacteriaCount.GetLength(0) * bacteriaCount.GetLength(1));
        Console.WriteLine($"Среднее число бактерий: {srednee}");

        // 8. Свободные клетки агара '.' во внутреннем круге
        int svobodno = 0;
        for (int i = 1; i < petriDish.GetLength(0) - 1; i++)
        {
            for (int j = 1; j < petriDish.GetLength(1) - 1; j++)
            {
                if (petriDish[i, j] == '.')
                    svobodno++;
            }
        }
        Console.WriteLine($"Свободных клеток агара внутри: {svobodno}");

        // Аналитический блок: суммы по строкам
        Console.WriteLine();
        Console.WriteLine("Суммы по строкам:");
        for (int i = 0; i < bacteriaCount.GetLength(0); i++)
        {
            int rowSum = 0;
            for (int j = 0; j < bacteriaCount.GetLength(1); j++)
            {
                rowSum += bacteriaCount[i, j];
            }
            Console.WriteLine($"Строка {i + 1}: суммарная численность = {rowSum}");
        }

        // Суммы по столбцам
        Console.WriteLine();
        Console.WriteLine("Суммы по столбцам:");
        for (int j = 0; j < bacteriaCount.GetLength(1); j++)
        {
            int colSum = 0;
            for (int i = 0; i < bacteriaCount.GetLength(0); i++)
            {
                colSum += bacteriaCount[i, j];
            }
            Console.WriteLine($"Столбец {j + 1}: суммарная численность = {colSum}");
        }

        // Глобальный экстремум по всей матрице
        int globalMax = -1;
        int gMaxI = -1;
        int gMaxJ = -1;

        for (int i = 0; i < bacteriaCount.GetLength(0); i++)
        {
            for (int j = 0; j < bacteriaCount.GetLength(1); j++)
            {
                if (bacteriaCount[i, j] > globalMax)
                {
                    globalMax = bacteriaCount[i, j];
                    gMaxI = i;
                    gMaxJ = j;
                }
            }
        }

        Console.WriteLine();
        Console.WriteLine($"Глобальный максимум: {globalMax} в клетке [{gMaxI},{gMaxJ}]");

        // Сканирование главной диагонали
        int mainDiagSum = 0;
        for (int i = 0; i < bacteriaCount.GetLength(0) &&
                        i < bacteriaCount.GetLength(1); i++)
        {
            mainDiagSum += bacteriaCount[i, i];
        }
        Console.WriteLine($"Сумма по главной диагонали: {mainDiagSum}");

        // Сканирование побочной диагонали
        int sideDiagSum = 0;
        for (int i = 0; i < bacteriaCount.GetLength(0) &&
                        i < bacteriaCount.GetLength(1); i++)
        {
            sideDiagSum += bacteriaCount[i, bacteriaCount.GetLength(1) - 1 - i];
        }
        Console.WriteLine($"Сумма по побочной диагонали: {sideDiagSum}");

        // 9. Внесение антибиотика с проверкой координат
        Console.WriteLine();
        Console.Write("Введите строку антибиотика: ");
        int x = int.Parse(Console.ReadLine());

        Console.Write("Введите столбец антибиотика: ");
        int y = int.Parse(Console.ReadLine());

        if (x >= 0 && x < rows && y >= 0 && y < cols)
        {
            petriDish[x, y] = 'A';
            bacteriaCount[x, y] = 0;

            // Сосед слева по горизонтали
            if (y - 1 >= 0)
            {
                petriDish[x, y - 1] = 'A';
                bacteriaCount[x, y - 1] = 0;
            }

            // Сосед справа по горизонтали
            if (y + 1 < cols)
            {
                petriDish[x, y + 1] = 'A';
                bacteriaCount[x, y + 1] = 0;
            }
        }
        else
        {
            Console.WriteLine("Координаты вне чашки!");
        }

        // 10. Главная диагональ — полоса стерилизации
        for (int i = 0; i < petriDish.GetLength(0) &&
                        i < petriDish.GetLength(1); i++)
        {
            petriDish[i, i] = 'X';
            bacteriaCount[i, i] = 0;
        }

        // Итоговый планшет
        Console.WriteLine();
        Console.WriteLine("Итоговый планшет:");
        for (int i = 0; i < petriDish.GetLength(0); i++)
        {
            for (int j = 0; j < petriDish.GetLength(1); j++)
            {
                Console.Write(petriDish[i, j]);
                if (j < petriDish.GetLength(1) - 1)
                    Console.Write(":");
            }
            Console.WriteLine();
        }

        // Итоговая численность
        Console.WriteLine();
        Console.WriteLine("Численность бактерий:");
        for (int i = 0; i < bacteriaCount.GetLength(0); i++)
        {
            for (int j = 0; j < bacteriaCount.GetLength(1); j++)
            {
                Console.Write(bacteriaCount[i, j]);
                if (j < bacteriaCount.GetLength(1) - 1)
                    Console.Write(":");
            }
            Console.WriteLine();
        }
    }
}
```
