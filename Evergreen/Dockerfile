#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

# STAGE0 - Configure web app reqs
FROM mcr.microsoft.com/dotnet/core/aspnet:3.1-buster-slim AS base
WORKDIR /app
EXPOSE 80

# STAGE01 - Build application and its dependencies
FROM mcr.microsoft.com/dotnet/core/sdk:3.1-buster AS build
WORKDIR /src
COPY ["Evergreen.csproj", ""]
RUN dotnet restore "./Evergreen.csproj"
COPY . .
WORKDIR "/src/."
RUN dotnet build "Evergreen.csproj" -c Release -o /app/build

# STAGE02 - Publish the application
FROM build AS publish
RUN dotnet publish "Evergreen.csproj" -c Release -o /app/publish

# STAGE03 - Create the final image
FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Evergreen.dll"]