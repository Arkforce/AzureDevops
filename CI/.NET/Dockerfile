# Use the official .NET SDK image to build the app
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
WORKDIR /app
EXPOSE 80

# Use the .NET SDK image to build the app
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /src
COPY ["MyDotNetWEBAPP.csproj", "./"]
RUN dotnet restore "MyDotNetWEBAPP.csproj"
COPY . .
WORKDIR "/src"
RUN dotnet build "MyDotNetWEBAPP.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "MyDotNetWEBAPP.csproj" -c Release -o /app/publish

# Final image with just the runtime
FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENV ASPNETCORE_URLS=http://+:80
ENTRYPOINT ["dotnet", "MyDotNetWEBAPP.dll"]
