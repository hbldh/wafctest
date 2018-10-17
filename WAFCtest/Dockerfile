#Depending on the operating system of the host machines(s) that will build or run the containers, the image specified in the FROM statement may need to be changed.
#For more information, please see https://aka.ms/containercompat

FROM microsoft/aspnet:4.7.1-windowsservercore-ltsc2016 AS base
WORKDIR /app
EXPOSE 50026
EXPOSE 44336

FROM microsoft/dotnet-framework:4.7.1-sdk-windowsservercore-ltsc2016 AS build
WORKDIR /src
COPY ["WAFCtest/WAFCtest.csproj", "WAFCtest/"]
RUN dotnet restore "WAFCtest/WAFCtest.csproj"
COPY . .
WORKDIR "/src/WAFCtest"
RUN dotnet build "WAFCtest.csproj" -c Release -o /app

FROM build AS publish
RUN dotnet publish "WAFCtest.csproj" -c Release -o /app

FROM base AS final
WORKDIR /app
COPY --from=publish /app .
ENV ASPNETCORE_URLS="http://*:5000"
ENV ASPNETCORE_ENVIRONMENT="Production"
ENTRYPOINT ["WAFCtest.exe"]