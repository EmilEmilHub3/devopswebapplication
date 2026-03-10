# Build stage
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /src

# Copy project file and restore dependencies
COPY DevOpsWebApplication/DevOpsWebApplication.csproj DevOpsWebApplication/
RUN dotnet restore DevOpsWebApplication/DevOpsWebApplication.csproj

# Copy the rest of the source code
COPY . .

# Publish the application
RUN dotnet publish DevOpsWebApplication/DevOpsWebApplication.csproj -c Release -o /app/publish /p:UseAppHost=false

# Runtime stage
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS final
WORKDIR /app

COPY --from=build /app/publish .

# ASP.NET Core listens on port 8080 inside the container
ENV ASPNETCORE_URLS=http://+:8080
EXPOSE 8080

ENTRYPOINT ["dotnet", "DevOpsWebApplication.dll"]