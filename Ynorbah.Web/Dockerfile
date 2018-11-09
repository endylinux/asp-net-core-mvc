#Depending on the operating system of the host machines(s) that will build or run the containers, the image specified in the FROM statement may need to be changed.
#For more information, please see https://aka.ms/containercompat

FROM microsoft/dotnet:2.1-aspnetcore-runtime-nanoserver-1803 AS base
WORKDIR /app
EXPOSE 55633
EXPOSE 44379

FROM microsoft/dotnet:2.1-sdk-nanoserver-1803 AS build
WORKDIR /src
COPY ["asp-net-core-mvc/asp-net-core-mvc.csproj", "asp-net-core-mvc/"]
RUN dotnet restore "asp-net-core-mvc/asp-net-core-mvc.csproj"
COPY . .
WORKDIR "/src/asp-net-core-mvc"
RUN dotnet build "asp-net-core-mvc.csproj" -c Release -o /app

FROM build AS publish
RUN dotnet publish "asp-net-core-mvc.csproj" -c Release -o /app

FROM base AS final
WORKDIR /app
COPY --from=publish /app .
ENTRYPOINT ["dotnet", "asp-net-core-mvc.dll"]