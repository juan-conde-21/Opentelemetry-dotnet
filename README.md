# Opentelemetry-dotnet

Construir la imagen del microservicio, para ello agregamos las dependencias y configuracion a nivel del codigo del microservicio.

1. Instalar las librerias de Opentelemetry en el proyecto del microservicio.

dotnet add package OpenTelemetry.Exporter.Console
dotnet add package OpenTelemetry.Exporter.OpenTelemetryProtocol
dotnet add package OpenTelemetry.Instrumentation.Http --prerelease

2. Importar las librerias a nivel del codigo de la aplicacion.

using OpenTelemetry;
using OpenTelemetry.Resources;
using OpenTelemetry.Trace;

3. Declarar los detalles del microservicio dentro del codigo de la aplicación.

        using var tracerProvider = Sdk.CreateTracerProviderBuilder()
            .SetResourceBuilder(ResourceBuilder.CreateDefault().AddService(
                serviceName: "DemoApp",
                serviceVersion: "1.0.0"))
            .AddSource("OpenTelemetry.Demo.Jaeger")
            .AddHttpClientInstrumentation()
            .AddConsoleExporter()
            .AddOtlpExporter()
            .Build();


4. Construir la imagen, para este ejemplo utilizamos los archivos dentro de la carpte app/ , para ello ejecutar el siguiente comando.

Docker build -t program:latest .


5. 




