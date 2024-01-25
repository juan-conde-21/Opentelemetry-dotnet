FROM mcr.microsoft.com/dotnet/sdk:8.0

WORKDIR /app

RUN dotnet new console --output dotnetapp

WORKDIR /app/dotnetapp

RUN dotnet add package OpenTelemetry.Exporter.Console
RUN dotnet add package OpenTelemetry.Exporter.OpenTelemetryProtocol
RUN dotnet add package OpenTelemetry.Instrumentation.Http --prerelease


COPY Program.cs .

CMD ["dotnet","run"]
