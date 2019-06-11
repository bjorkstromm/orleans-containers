FROM mcr.microsoft.com/dotnet/core/aspnet:2.1 AS base
WORKDIR /app
EXPOSE 80

FROM mcr.microsoft.com/dotnet/core/sdk:2.1 AS build
WORKDIR /src
COPY GrainInterfaces/GrainInterfaces.csproj GrainInterfaces/
COPY API/API.csproj API/
COPY Directory.Build.props .
RUN dotnet restore API/API.csproj
COPY . .
WORKDIR /src/API
RUN dotnet build --no-restore -c Release -o /app

FROM build AS publish
RUN dotnet publish --no-restore -c Release -o /app

FROM base AS final
WORKDIR /app
COPY --from=publish /app .
ENTRYPOINT ["dotnet", "API.dll"]
