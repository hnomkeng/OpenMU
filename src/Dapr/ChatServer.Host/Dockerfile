FROM mcr.microsoft.com/dotnet/aspnet:7.0-alpine AS base
WORKDIR /app
EXPOSE 80
EXPOSE 55980

FROM mcr.microsoft.com/dotnet/sdk:7.0-alpine AS build
WORKDIR /src
COPY ["Dapr/ChatServer.Host/MUnique.OpenMU.ChatServer.Host.csproj", "Dapr/ChatServer.Host/"]
RUN dotnet restore "Dapr/ChatServer.Host/MUnique.OpenMU.ChatServer.Host.csproj"
COPY . .
WORKDIR "/src/Dapr/ChatServer.Host"
RUN dotnet build "MUnique.OpenMU.ChatServer.Host.csproj" -c Release -o /app/build -p:ci=true

FROM build AS publish
RUN dotnet publish "MUnique.OpenMU.ChatServer.Host.csproj" -c Release -o /app/publish -p:ci=true

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "MUnique.OpenMU.ChatServer.Host.dll"]
