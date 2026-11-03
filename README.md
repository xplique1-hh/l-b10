# Домашнее задание к работе 10

## Условие задачи
Составьте программу, которая для заданной пользователем фигуры (прямоугольного треугольника) предлагает меню выбора одной из операций:
1. Рассчитать площадь.
2. Вывести определение фигуры.
3. Нарисовать фигуру.
4. Сравнить две фигуры по площади.

Для заданной фигуры необходимо реализовать функции:
- Вычисления площади.
- Рисования фигуры заданным символом (размеры задаются в `main`).
- Вывода параметров фигуры.
- Сравнения двух фигур по площади.

## 1. Алгоритм

### Алгоритм
1. **Начало**
2. Установить русскую локаль для корректного вывода текста.
3. Запросить у пользователя исходные данные:
   - Символ для рисования фигуры.
   - Параметры первого треугольника (острый угол `a1` и прилежащий катет `b1`).
   - Параметры второго треугольника (острый угол `a2` и прилежащий катет `b2`).
4. Организовать цикл для вывода меню и обработки выбора пользователя (do-while).
5. Вывести пункты меню и запросить выбор (`choice`).
6. В зависимости от выбора (`switch`):
   - **1**: Вызвать функцию вычисления и вывода площади первого треугольника.
   - **2**: Вызвать функцию вывода текстового определения прямоугольного треугольника.
   - **3**: Вызвать функцию отрисовки треугольника символами (расчет высоты по углу и катету, затем построчный вывод).
   - **4**: Вызвать функцию сравнения двух треугольников по площади.
   - **5**: Запросить новые параметры для треугольников.
   - **0**: Завершить программу.
7. Если введен неверный пункт, вывести сообщение об ошибке и вернуться к п. 5.
8. **Конец**

## 2. Реализация программы

```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
#include <math.h>
#include <locale.h>

const double PI = 3.14159265358979323846;

float leg_a(float a, float b)
{
    return (float)(b * tan(a * PI / 180.0));
}

float area_triangle(float a, float b)
{
    return leg_a(a, b) * b / 2;
}
int get_height_symbols(float a, int b)
{
    int h = (int)(leg_a(a, (float)b) + 0.5);
    if (h < 1) h = 1;
    return h;
}
float compare_triangle(float a1, float b1, float a2, float b2)
{
    return area_triangle(a1, b1) - area_triangle(a2, b2);
}
void draw_triangle(int b, int h, char sym)
{
    int r, c;
    for (r = 0; r < h; r++)
    {
        int n = (int)((double)b * (r + 1) / h + 0.5);
        if (n < 1) n = 1;
        for (c = 0; c < b - n; c++)
            putchar(' ');
        for (c = 0; c < n; c++)
            putchar(sym);
        putchar('\n');
    }
}

void show_area(float a, int b)
{
    printf("Катет a = %.2f\n", leg_a(a, (float)b));
    printf("Площадь S = %.2f\n", area_triangle(a, (float)b));
}

void print_definition(void)
{
    printf("\nПрямоугольный треугольник - это треугольник, у которого один угол прямой (90 градусов).\n");
    printf("Стороны, образующие прямой угол, называются катетами, а сторона напротив\n");
    printf("прямого угла - гипотенузой.\n");
    printf("В программе треугольник задаётся острым углом a и прилежащим катетом b.\n");
    printf("Противолежащий катет: a = b * tg(угла), площадь: S = a * b / 2.\n");
}

// Вывод фигуры на экран
void show_figure(float a, int b, char sym)
{
    int h = get_height_symbols(a, b);
    printf("Высота (катет a) = %d симв.\n\n", h);
    draw_triangle(b, h, sym);
}

void show_compare(float a1, int b1, float a2, int b2)
{
    float diff = compare_triangle(a1, (float)b1, a2, (float)b2);
    printf("S1 = %.2f, S2 = %.2f\n", area_triangle(a1, (float)b1), area_triangle(a2, (float)b2));
    printf("Разница S1 - S2 = %.2f\n", diff);
    if (diff > 0)
        printf("Первый треугольник больше\n");
    else if (diff < 0)
        printf("Второй треугольник больше\n");
    else
        printf("Площади равны\n");
}

float input_angle(void)
{
    float x;
    do {
        printf("Введите угол a в градусах (от 1 до 89): ");
        scanf("%f", &x);
    } while (x <= 0 || x >= 90);

    return x;
}

int input_leg(void)
{
    int x;
    do {
        printf("Введите прилежащий катет b (в символах, > 0): ");
        scanf("%d", &x);
    } while (x <= 0);
    return x;
}

void input_params(char* sym, float* a1, int* b1, float* a2, int* b2)
{
    printf("Введите символ из которого будет строиться фигура: ");
    scanf(" %c", sym);
    printf("--- Первый треугольник ---\n");
    *a1 = input_angle();
    *b1 = input_leg();
    printf("--- Второй треугольник (для сравнения) ---\n");
    *a2 = input_angle();
    *b2 = input_leg();
}

void print_menu(void)
{
    printf("1 - Рассчитать площадь\n");
    printf("2 - Вывести определение фигуры\n");
    printf("3 - Нарисовать фигуру\n");
    printf("4 - Сравнить два треугольника по площади\n");
    printf("5 - Ввести новые параметры\n");
    printf("0 - Выход\n");
    printf("Ваш выбор: ");
}

int main(void)
{
    int choice;
    char sym;
    float alpha1, alpha2;
    int b1, b2;
    setlocale(LC_ALL, "Russian");
    input_params(&sym, &alpha1, &b1, &alpha2, &b2);
    do
    {
        print_menu();
        scanf("%d", &choice);
        switch (choice)
        {
        case 1:
            show_area(alpha1, b1);
            break;
        case 2:
            print_definition();
            break;
        case 3:
            show_figure(alpha1, b1, sym);
            break;
        case 4:
            show_compare(alpha1, b1, alpha2, b2);
            break;
        case 5:
            input_params(&sym, &alpha1, &b1, &alpha2, &b2);
            break;
        case 0:
            printf("Выход из программы.\n");
            break;
        default:
            printf("Неверный пункт меню, попробуйте ещё раз.\n");
        }
    } while (choice != 0);
    return 0;
}
```

## 3. Результаты работы программы
<img width="943" height="1038" alt="image" src="https://github.com/user-attachments/assets/5c4190dc-40f2-4c43-9aa8-45196538f3b1" />

## 4. Информация о разработчике

Сергеенков Дмитрий бИЦТ-261
