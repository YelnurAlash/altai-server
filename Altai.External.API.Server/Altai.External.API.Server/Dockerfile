FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
USER $APP_UID
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["Altai.External.API.Server/Altai.External.API.Server.csproj", "Altai.External.API.Server/"]
RUN dotnet restore "Altai.External.API.Server/Altai.External.API.Server.csproj"
COPY . .
WORKDIR "/src/Altai.External.API.Server"
RUN dotnet build "Altai.External.API.Server.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "Altai.External.API.Server.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Altai.External.API.Server.dll"]
