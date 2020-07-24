#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/core/aspnet:3.1-buster-slim AS base
WORKDIR /app
EXPOSE 443

FROM mcr.microsoft.com/dotnet/core/sdk:3.1-buster AS build
WORKDIR /src
COPY ["AppNotifier.Api/AppNotifier.Api.csproj", "AppNotifier.Api/"]
RUN dotnet restore "AppNotifier.Api/AppNotifier.Api.csproj"
COPY . .
WORKDIR "/src/AppNotifier.Api"
RUN dotnet build "AppNotifier.Api.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "AppNotifier.Api.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "AppNotifier.Api.dll"]