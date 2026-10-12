```dockerfile
# Этап 1. Сборка, здесь нужен SDK
FROM mcr.microsoft.com/dotnet/sdk:10.0 AS build
WORKDIR /src

# Сначала только файл проекта, чтобы сработал кэш слоев
COPY *.csproj .
RUN dotnet restore

# Теперь весь остальной код
COPY . .

# Публикуем приложение в папку /app/publish
RUN dotnet publish -c Release -o /app/publish --no-restore

# Этап 2. Запуск, здесь достаточно runtime
FROM mcr.microsoft.com/dotnet/runtime:10.0
WORKDIR /app

# Копируем готовую сборку из этапа build
COPY --from=build /app/publish .

# Команда запуска
ENTRYPOINT ["dotnet", "MyApp.dll"]
```

### Разбор этапа 1. Сборка

|Строка|Что делает|Зачем|
|---|---|---|
|`FROM mcr.microsoft.com/dotnet/sdk:10.0 AS build`|Берем образ с полным .NET 10 SDK (весит ~1.5 ГБ) и даем ему имя build|Нужен компилятор, dotnet restore, dotnet publish|
|`WORKDIR /src`|Создаем папку /src внутри контейнера и переходим в нее|Все дальнейшие команды будут выполняться в /src|
|`COPY *.csproj .`|Копируем файл проекта (.csproj) из хоста в /src|dotnet restore нужно знать зависимости|
|`RUN dotnet restore`|Восстанавливаем NuGet-пакеты|Отдельным шагом, чтобы Docker закэшировал слой. Пока .csproj не меняется, restore не запускается заново|
|`COPY . .`|Копируем весь остальной код (все .cs файлы, ресурсы и прочее)|Нужно для компиляции|
|`RUN dotnet publish -c Release -o /app/publish --no-restore`|Компилируем и публикуем приложение в папку /app/publish|Флаг --no-restore не восстанавливает пакеты заново, мы уже сделали это выше. На выходе готовая DLL|

### Разбор этапа 2. Запуск

|Строка|Что делает|Зачем|
|---|---|---|
|`FROM mcr.microsoft.com/dotnet/runtime:10.0`|Берем легкий образ только с .NET Runtime (весит ~200 МБ)|Для запуска не нужен компилятор|
|`WORKDIR /app`|Создаем рабочую папку /app|Сюда положим наше приложение|
|`COPY --from=build /app/publish .`|Копируем файлы из предыдущего этапа (из папки /app/publish этапа build) в текущую папку /app|Магия многостадийной сборки, мы берем результат компиляции, не таща за собой SDK|
|`ENTRYPOINT ["dotnet", "MyApp.dll"]`|Задаем команду запуска|Когда контейнер стартует, выполнится dotnet MyApp.dll|

