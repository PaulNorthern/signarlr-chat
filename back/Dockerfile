# # Use the official Microsoft .NET Core SDK image as a build image.
# # This image includes the necessary tools to build a .NET Core application.
# FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
# COPY . .
# WORKDIR /App
# RUN dotnet restore
# RUN dotnet publish -c Release -o out

# # Build Runtime image
# # Use the official Microsoft .NET Core runtime image as a runtime image.
# # This image includes only the necessary components to run a .NET Core application.
# FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS runtime
# WORKDIR /App
# # Copy the published application files into the container and set the entrypoint.
# COPY --from=build /App/out .
# EXPOSE 80
# ENTRYPOINT ["dotnet", "back.dll"]

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build-env
WORKDIR /app

# Копируем csproj и восстанавливаем зависимости
COPY *.csproj ./
RUN dotnet restore

# Копируем остальные файлы проекта и собираем
COPY . ./
RUN dotnet publish -c Release -o out

# Разделяем собранный файл на отдельный слой
FROM mcr.microsoft.com/dotnet/aspnet:6.0
WORKDIR /app
COPY --from=build-env /app/out .

ENTRYPOINT ["dotnet", "back.dll"]
