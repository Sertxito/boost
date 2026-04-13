# .NET Backend Standards

## Objetivo

Estandarizar backend .NET para mantener consistencia, observabilidad y mantenibilidad.

## 1. Program.cs recomendado

Separar el registro por extension methods para mantener Program.cs limpio.

```csharp
var builder = WebApplication.CreateBuilder(args);

builder.Services
    .AddApi(builder.Configuration)
    .AddApplication()
    .AddInfrastructure(builder.Configuration)
    .AddObservability(builder.Configuration)
    .AddApiDocumentation();

var app = builder.Build();

app.UseExceptionHandler();
app.UseHttpsRedirection();
app.UseAuthentication();
app.UseAuthorization();

if (app.Environment.IsDevelopment())
{
    app.UseSwagger();
    app.UseSwaggerUI();
}

app.MapControllers();
app.MapHealthChecks("/health");

app.Run();
```

## 2. Inyeccion de dependencias (DI)

- Singleton: servicios sin estado y thread-safe.
- Scoped: servicios por request (ejemplo: unidad de trabajo, contexto de datos).
- Transient: servicios livianos y sin estado compartido.
- Evitar Service Locator y resolver dependencias manualmente.

Ejemplo de registro por capa:

```csharp
public static class ServiceCollectionExtensions
{
    public static IServiceCollection AddApplication(this IServiceCollection services)
    {
        services.AddScoped<IOrderService, OrderService>();
        services.AddScoped<ICommandHandler<CreateOrderCommand>, CreateOrderCommandHandler>();
        return services;
    }
}
```

## 3. Swagger/OpenAPI completo

Configurar metadata, XML comments y seguridad.

```csharp
services.AddEndpointsApiExplorer();
services.AddSwaggerGen(options =>
{
    options.SwaggerDoc("v1", new Microsoft.OpenApi.Models.OpenApiInfo
    {
        Title = "My API",
        Version = "v1",
        Description = "API de negocio",
        Contact = new Microsoft.OpenApi.Models.OpenApiContact
        {
            Name = "Backend Team",
            Email = "backend@example.com"
        }
    });

    var securityScheme = new Microsoft.OpenApi.Models.OpenApiSecurityScheme
    {
        Name = "Authorization",
        Type = Microsoft.OpenApi.Models.SecuritySchemeType.Http,
        Scheme = "bearer",
        BearerFormat = "JWT",
        In = Microsoft.OpenApi.Models.ParameterLocation.Header,
        Reference = new Microsoft.OpenApi.Models.OpenApiReference
        {
            Type = Microsoft.OpenApi.Models.ReferenceType.SecurityScheme,
            Id = "Bearer"
        }
    };

    options.AddSecurityDefinition("Bearer", securityScheme);
    options.AddSecurityRequirement(new Microsoft.OpenApi.Models.OpenApiSecurityRequirement
    {
        { securityScheme, Array.Empty<string>() }
    });
});
```

## 4. Clases atomicas y estructura

- Una clase, una responsabilidad.
- No mezclar DTOs con entidades de dominio.
- No meter clases dentro de otras salvo razon fuerte y documentada.

Estructura sugerida:

- src/MyApi.Api
- src/MyApi.Application
- src/MyApi.Domain
- src/MyApi.Infrastructure

## 5. Helpers y genericos con criterio

- Extraer helpers para utilidades transversales repetidas.
- Mantener reglas de negocio en Domain/Application.
- Usar genericos para eliminar duplicacion real, no por moda.
