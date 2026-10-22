В `C# 7.2` был введен модификатор `in`, который позволяет передавать параметры по ссылке, но при этом гарантировать, что они не будут изменены в методе. Это позволяет избежать накладных расходов на копирование, сохраняя неизменяемость данных
```csharp
struct LargeStruct
{
    public int Value1;
    public int Value2;
    // Множество других полей...
}

static void ProcessLargeStruct(in LargeStruct largeStruct)
{
    largeStruct.Value1++; // ошибка компиляции: изменить largeStruct невозможно
}

static void Main()
{
    var largeStruct = new LargeStruct();
    ProcessLargeStruct(in largeStruct); // здесь in указывать необязательно 
}
```
При вызове метода, принимающего параметры с модификатором `in`, использовать этот модификатор при отправке параметра необязательно:
```csharp
static void Main()
{
    var largeStruct = new LargeStruct();
    ProcessLargeStruct(largeStruct); // отправка параметра без использования in 
}
```
