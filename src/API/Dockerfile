FROM mcr.microsoft.com/dotnet/sdk:7.0 AS build
WORKDIR /source
COPY . .
RUN dotnet restore "API.csproj" --disable-parallel
RUN dotnet publish "API.csproj" -c release -o /app --no-restore

FROM mcr.microsoft.com/dotnet/aspnet:7.0
WORKDIR /app
COPY --from=build /app ./

ENV ASPNETCORE_URLS=http://+:5000 
EXPOSE 5000

ENTRYPOINT [ "dotnet", "API.dll"]