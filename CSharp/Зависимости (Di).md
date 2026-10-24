### Что это
**Внедрение зависимостей (Dependency Injection, DI)** — это шаблон проектирования в объектно-ориентированном программировании, при котором объект получает свои зависимости (другие объекты, от которых он зависит) извне, а не создаёт их самостоятельно. Это позволяет снизить связанность между компонентами системы, повысить их модульность, тестируемость и переиспользуемость.

### Добавляются
В Asp в классе Program.cs:
``` c#
using BinnoMetric.DataBase;
using BinnoMetric.Service;
using Microsoft.EntityFrameworkCore;
using Microsoft.OpenApi;

var builder = WebApplication.CreateBuilder(args);

builder.Services.AddControllers();
builder.Services.AddOpenApi();
builder.Services.AddSwaggerGen(
    options =>
    {
        var basePath = AppContext.BaseDirectory;
        var xmlPath = Path.Combine(basePath, "BinnoMetricDoc.xml");
        options.IncludeXmlComments(xmlPath, includeControllerXmlComments: true);
        options.SwaggerDoc("v1", new OpenApiInfo
        {
            Version = "v1",
            Title = "BinnoMetric",
            Description = "ASP.NET Core Web API для учёта фармацевтического производства"
        });
    }
);
string connectionString = builder.Configuration.GetConnectionString("DefaultConnection");
builder.Services.AddDbContext<BinnoDBContext>(options => options.UseSqlServer(connectionString));

builder.Services.AddTransient<EmployeesService>();
builder.Services.AddTransient<IntegrationService>();
builder.Services.AddTransient<ProductService>();
builder.Services.AddTransient<DowntimeTypeService>();
builder.Services.AddTransient<DowntimeRecordService>();
builder.Services.AddTransient<ProductionRecordsService>();
builder.Services.AddTransient<AnalyticsService>();
builder.Services.AddTransient<EquipmentLineService>();
builder.Services.AddCors(options =>
...
var app = builder.Build();
```
В Maui в классе MauiProgram.cs:
``` c#
using DBLibrary.Data;
using LiveChartsCore.SkiaSharpView.Maui;
using Microsoft.Extensions.Logging;
using Sigma_Trainer.Services;
using Sigma_Trainer.View;
using Sigma_Trainer.ViewModel;
using SkiaSharp.Views.Maui.Controls.Hosting;

namespace Sigma_Trainer
{
    public static class MauiProgram
    {
        public static MauiApp CreateMauiApp()
        {
            var builder = MauiApp.CreateBuilder();
            builder
                .UseMauiApp<App>()
                .UseSkiaSharp()
                .UseLiveCharts()
                .ConfigureFonts(fonts =>
                {
                    fonts.AddFont("OpenSans-Regular.ttf", "OpenSansRegular");
                    fonts.AddFont("OpenSans-Semibold.ttf", "OpenSansSemibold");
                });
            //База дынных
            builder.Services.AddDbContext<SigmaTrainerDbContext>();
            builder.Services.AddSingleton<FoodService>();
            builder.Services.AddSingleton<StatisticsService>();
            builder.Services.AddSingleton<ExerciseService>();
            //Вкладки
            builder.Services.AddSingleton<SummaryPage>();
            builder.Services.AddSingleton<SummaryViewModel>();
            builder.Services.AddSingleton<WorkoutPage>();
            builder.Services.AddSingleton<WorkoutViewModel>();
            builder.Services.AddSingleton<NutritionPage>();
            builder.Services.AddSingleton<NutritionViewModel>();
            builder.Services.AddSingleton<AddFoodRecordPage>();
            builder.Services.AddSingleton<AddFoodRecordViewModel>();
            builder.Services.AddSingleton<AddExercisePage>();
            builder.Services.AddSingleton<AddExerciseViewModel>();
            builder.Services.AddSingleton<SettingsPage>();
            builder.Services.AddSingleton<SettingsViewModel>();
            builder.Services.AddSingleton<SettingsService>();
#if DEBUG
            builder.Logging.AddDebug();
#endif

            return builder.Build();
        }
    }
}
```
### Используются
Di контейнер получается через конструктор
```c#
public class AnalyticsController : ControllerBase
{
    private readonly AnalyticsService _analyticsService;
    public AnalyticsController(AnalyticsService analyticsService)
    {
        _analyticsService = analyticsService;
    }
```