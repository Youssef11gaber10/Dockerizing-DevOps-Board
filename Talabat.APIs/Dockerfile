#See https://aka.ms/customizecontainer to learn how to customize your debug container and how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["Talabat.APIs/Talabat.APIs.csproj", "Talabat.APIs/"]
COPY ["Talabat.Repository/Talabat.Repository.csproj", "Talabat.Repository/"]
COPY ["Talabat.Core/Talabat.Core.csproj", "Talabat.Core/"]
COPY ["Talabat.Service/Talabat.Service.csproj", "Talabat.Service/"]
RUN dotnet restore "./Talabat.APIs/./Talabat.APIs.csproj"
COPY . .
WORKDIR "/src/Talabat.APIs"
RUN dotnet build "./Talabat.APIs.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "./Talabat.APIs.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Talabat.APIs.dll"]