FROM mcr.microsoft.com/dotnet/core/sdk:3.0 AS build-env
WORKDIR /app

# Copy necessary files and restore as distinct layer
COPY *.csproj ./
RUN dotnet restore -s https://api.nuget.org/v3/index.json -s https://www.myget.org/F/pitstop/api/v3/index.json

# Copy everything else and build
COPY . ./
RUN dotnet publish -c Release -o out

# Build runtime image
FROM mcr.microsoft.com/dotnet/core/aspnet:3.0
COPY --from=build-env /app/out .

# Expose ports
EXPOSE 5000/tcp
ENV ASPNETCORE_URLS http://*:5000
HEALTHCHECK --interval=30s --timeout=3s --retries=1 CMD curl --silent --fail http://localhost:5000/hc || exit 1

# Start
ENTRYPOINT ["dotnet", "Pitstop.VehicleManagementAPI.dll"]
