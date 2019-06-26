---
title: "Dockerizing Aspnet Core App"
date: 2019-06-26T06:56:18-04:00
draft: true
---

Assuming that you have the correct docker environment, asp.net core app can run in a container using the following:

{{< highlight docker >}}
FROM mcr.microsoft.com/dotnet/core/sdk:2.2 AS build-env
WORKDIR /app

# Copy csproj and restore as distinct layers
COPY *.csproj ./
RUN dotnet restore

# Copy everything else and build
COPY . ./
RUN dotnet publish -c Release -o out

# Build runtime image
FROM mcr.microsoft.com/dotnet/core/aspnet:2.2
WORKDIR /app
COPY --from=build-env /app/out .
# RUN ["ls -alt"]
ENTRYPOINT ["dotnet", "DemoApp.dll"]%
{{< /highlight >}}
