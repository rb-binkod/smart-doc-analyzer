# Use specific platform to avoid architecture mismatch
FROM --platform=linux/amd64 mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /src

# Copy entire solution
COPY . .

# Restore using the solution file (ensures dependencies are found)
RUN dotnet restore "SmartDocAnalyzer.sln"

# Publish only the API project
RUN dotnet publish "SmartDocAnalyzer.API/SmartDocAnalyzer.API.csproj" -c Release -o /app/publish

# Build runtime image
FROM --platform=linux/amd64 mcr.microsoft.com/dotnet/aspnet:8.0 AS runtime
WORKDIR /app
COPY --from=build /app/publish .
EXPOSE 80
EXPOSE 443
ENTRYPOINT ["dotnet", "SmartDocAnalyzer.API.dll"]
