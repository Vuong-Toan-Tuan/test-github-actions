FROM mcr.microsoft.com/dotnet/aspnet:9.0 AS base
USER $APP_UID
WORKDIR /app
ENV ASPNETCORE_URLS=http://+:5106
EXPOSE 8088
EXPOSE 8089

FROM mcr.microsoft.com/dotnet/sdk:9.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["./test-github-actions.csproj", "test-github-actions/"]
RUN dotnet restore "test-github-actions/test-github-actions.csproj"
COPY . test-github-actions/
WORKDIR "/src/test-github-actions"
RUN dotnet build "./test-github-actions.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "./test-github-actions.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "test-github-actions.dll"]
