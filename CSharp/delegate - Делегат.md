# Описание
Делегат - это объект, который может указывать на метод. То есть, с помощью делегата мы можем создавать переменные, которые содержат метод.
При создании делегаты мы определяем сигнатуру метода, который может быть записан в этот делегат.
# Синтаксис
Message - делегат с сигнатурой, которая ничего не принимает и не возвращает
SomeDel - делегат с сигнатурой, которая принимает 2 значения и ничего не возвращает
```csharp
delegate void Message();
delegate void SomeDel(int a, double b);
```
Что бы записать в делегат метод, мы должны создать метод с такой-же сигнатурой, как и у делегата.
```csharp
static void Hello() => Console.WriteLine("Hello, not METANIT.COM, but Obsidian");
static void SomeMethod(int a, double b) 
{
	Console.WriteLine($"Этот метод ничего не возвращает, но принимает {a} и {b}");
}
```
А теперь создадим методы, которые будут принимать в качестве аргументов другие методы (наши делегаты).
messageAction принимает любой метод, который соответствует сигнатуре делегата.
SomeMethodAction - так-же, но ещё 2 параметра (a и b)
```csharp
static void messageAction(Message message)
{
	message();
}
static void SomeMethodAction(SomeDel SomeDel, int a, double b)
{
	SomeDel(a, b);
}
```
А теперь вызовем наши методы:
Здесь messageAction принимает наш метод Hello, который соответствует сигнатуре делегата Message.
А метод SomeMethodAction принимает метод SomeMethod, который соответствует сигнатуре SomeDel. В методы мы пишет только название делегата!! А вот параметры, которые пойдут в SomeMethod мы передадим отдельно. Как видно выше, SomeMethodAction вызовет метод SomeDel и передаст туда параметры a и b
```csharp
messageAction(Hello);
SomeMethodAction(SomeMethod, 2, 3);
```
Данный код выведет:
```csharp
Hello, not METANIT.COM, but Obsidian
Этот метод ничего не возвращает, но принимает 2 и 3
```
Так же мы можем использовать анонимный метож:
```csharp
messageAction(() => Console.WriteLine("Это анонимный метод"));
// Выведет надписб "Это анонимный метод"
```
# Action
**Action** — предопределённый тип делегата в C#, который **инкапсулирует метод, который не возвращает значение** (возвращает void).  (Как messageAction выше).
## параметры
`Action<T>` - перегрузка делегата, который принимает 1 параметр и ничего не возвращает.
`Action<T, T2>` - перегрузка делегата, который принимает 2 параметра и ничего не возвращает.
`Action<T, T2, T3>` - перегрузка делегата, который принимает 3 параметра и ничего не возвращает.
`Action<T>` - перегрузка делегата, который ничего не принимает и ничего не возвращает.
```csharp
static void Main()
{
	messageAction(() => Console.WriteLine("Это анонимный метод"));
}

 // Этот метод принимает любые методы, 
//которые ничего не принимают и не возвращают
private static void messageAction(Action value)
{
	value();
}
```
Action - это просто вот такая строчка: 
public delegate void Action();
То есть, это абсолютно то-же самое, что мы писали в самом начале: 
delegate void Message(); // Отличаются только названия!

Ну и вот эти перегрузки методов:
```csharp
    private static void someMetod3(Action<int, int, int> value) { }
    private static void someMetod2(Action<int, int> value) { }
    private static void someMetod1(Action<int> value) { } 
```
Будут выглядеть вот так:
```csharp
public delegate void Action<in T1, in T2, in T3>(T1 arg1, T2 arg2, T3 arg3);
public delegate void Action<in T1, in T2>(T1 arg1, T2 arg2);
public delegate void Action<in T>(T obj);
```

# Func
**Action** — предопределённый тип делегата в C#, который **инкапсулирует метод, который возвращает что-то и может принимать или не принимать параметры. Возвращаемый параметр указывается последним.
## параметры
`Func<T1>` - перегрузка делегата, который возвращает 1 параметр.
`Func<T1, T2>` - перегрузка делегата, который возвращает параметр типа T2 и принимает параметр типа T1.
`Func<T1, T2, T3>` - перегрузка делегата, который возвращает параметр типа T3 и принимает параметры типа T1 и T2.
## Пример:
Здесь мы создаем некий метод MethodFromDelegate, 
который принимает делегат Func<int, string>.
В Main мы вызываем этот метод и в качестве параметра передаем другой метод: 
string SomeMethod(int a), который соответствует сигнатуре нашего делегата.
Внутри MethodFromDelegate происходит вызов переданного метода (SomeMethod).
func(5) - вот это и есть наш SomeMethod, принимающий int и возвращающий string
```csharp
class Program
{
    static void Main()
    {
        MethodFromDelegate(SomeMethod);
    }
    static string SomeMethod(int a)
    {
        return $"Этот метод принял число {a} и вернул текущую строку. a*a = {a * a}";
    } 
    static void MethodFromDelegate(Func<int, string> func)
    {
        Console.WriteLine(func(5));
    }
}
```