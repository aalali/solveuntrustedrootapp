# https://hub.docker.com/_/microsoft-dotnet
#From openjdk:latest AS base

FROM mcr.microsoft.com/dotnet/sdk:5.0-focal AS base
WORKDIR /app

# copy csproj and restore as distinct layers
FROM base AS build
WORKDIR /src
COPY solveuntrustedrootapp/*.csproj ./solveuntrustedrootapp/
RUN dotnet restore -r linux-x64

# copy everything else and build app
COPY solveuntrustedrootapp/. ./solveuntrustedrootapp/
WORKDIR /src/solveuntrustedrootapp
RUN dotnet publish -c release -o /app -r linux-x64 --self-contained false --no-restore

# final stage/image
FROM mcr.microsoft.com/dotnet/aspnet:5.0-focal-amd64
WORKDIR /app
COPY --from=build /app ./
ENTRYPOINT ["./solveuntrustedrootapp"]

