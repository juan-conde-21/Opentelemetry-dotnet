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


4. Construir la imagen, para este ejemplo utilizamos los archivos dentro de la carpeta app/ , para ello ejecutar el siguiente comando.

        Docker build -t dotnet:latest .

   Para este ejemplo se construyo y publico la siguiente imagen "juanconde24/dotnet".

5. Con la imagen construida se procede a desplegar la solucion en un cluster AKS.

   Crear el configmap configurado con los datos del tenant instana.

           kubectl create configmap config.yaml --from-file=config.yaml

   Desplegar el deployment del collector donde se hace la referencia al configmap previamente creado.

           kubectl apply -f collector.yaml

   Desplegar el deployment de la aplicación.

           kubectl apply -f deploy.yaml


6. Revisar las trazas recolectadas en el tenant instana.

![image](https://github.com/juan-conde-21/Opentelemetry-dotnet/assets/13276404/f97e478e-fcb5-4ce3-bedc-43c4e2e256c9)




