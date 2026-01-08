## Описание

Цикл `while` выполняет блок кода, пока условие истинно. Количество итераций может быть неизвестно заранее.

# Синтаксис
```csharp
int counter = 0;

while (counter < 5)
{
    Console.WriteLine($"Счетчик: {counter}");
    counter++;
}
```
## do-while
Выполняет тело цикла минимум один раз, затем проверяет условие.

```csharp

int number;
do
{
    Console.Write("Введите положительное число: ");
    number = Convert.ToInt32(Console.ReadLine());
} while (number <= 0);

Console.WriteLine($"Вы ввели: {number}");
```
