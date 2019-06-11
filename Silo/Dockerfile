FROM mcr.microsoft.com/dotnet/core/runtime:2.1 AS base
WORKDIR /app

FROM mcr.microsoft.com/dotnet/core/sdk:2.1 AS build
WORKDIR /src
COPY GrainInterfaces/GrainInterfaces.csproj GrainInterfaces/
COPY Grains/Grains.csproj Grains/
COPY Silo/Silo.csproj Silo/
COPY Directory.Build.props .
RUN dotnet restore Silo/Silo.csproj
COPY . .
WORKDIR /src/Silo
RUN dotnet build --no-restore -c Release -o /app

FROM build AS publish
RUN dotnet publish --no-restore -c Release -o /app

FROM base AS final
WORKDIR /app
COPY --from=publish /app .
ENTRYPOINT ["dotnet", "Silo.dll"]
