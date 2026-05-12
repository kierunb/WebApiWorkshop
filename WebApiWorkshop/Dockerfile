FROM mcr.microsoft.com/dotnet/aspnet:10.0 AS base
WORKDIR /app
EXPOSE 5033

ENV ASPNETCORE_URLS=http://+:5033

USER app
FROM --platform=$BUILDPLATFORM mcr.microsoft.com/dotnet/sdk:10.0 AS build
ARG configuration=Release
WORKDIR /src
COPY ["WebApiWorkshop/WebApiWorkshop/WebApiWorkshop.csproj", "WebApiWorkshop/WebApiWorkshop/"]
RUN dotnet restore "WebApiWorkshop/WebApiWorkshop/WebApiWorkshop.csproj"
COPY . .
WORKDIR "/src/WebApiWorkshop/WebApiWorkshop"
RUN dotnet build "WebApiWorkshop.csproj" -c $configuration -o /app/build

FROM build AS publish
ARG configuration=Release
RUN dotnet publish "WebApiWorkshop.csproj" -c $configuration -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "WebApiWorkshop.dll"]
