FROM --platform="linux/amd64" mcr.microsoft.com/dotnet/aspnet:8.0 AS base
USER $APP_UID
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

FROM --platform="linux/amd64" mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["HelloApplication/HelloApplication.csproj", "HelloApplication/"]
ENV DOTNET_NUGET_SIGNATURE_VERIFICATION=false
RUN dotnet restore "HelloApplication/HelloApplication.csproj"
COPY . .
WORKDIR "/src/HelloApplication"
RUN dotnet build "HelloApplication.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "HelloApplication.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "HelloApplication.dll"]
