# Stage 1 - Build
FROM mcr.microsoft.com/dotnet/sdk:9.0 AS build
WORKDIR /src

# Copy project and restore dependencies
COPY ./src/TodoApi/*.csproj ./TodoApi/
WORKDIR /src/TodoApi
RUN dotnet restore

# Copy the rest of the source code
COPY ./src/TodoApi/. .
RUN dotnet publish -c Release -o /app/publish

# Stage 2 - Runtime
FROM mcr.microsoft.com/dotnet/aspnet:9.0 AS runtime
WORKDIR /app
COPY --from=build /app/publish .

EXPOSE 5004
ENTRYPOINT ["dotnet", "TodoApi.dll"]
