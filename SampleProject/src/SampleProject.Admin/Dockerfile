FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["src/SampleProject.Admin/SampleProject.Admin.csproj", "src/SampleProject.Admin/"]
COPY ["src/SampleProject.Admin.EntityFramework.Shared/SampleProject.Admin.EntityFramework.Shared.csproj", "src/SampleProject.Admin.EntityFramework.Shared/"]
COPY ["src/SampleProject.Admin.EntityFramework.SqlServer/SampleProject.Admin.EntityFramework.SqlServer.csproj", "src/SampleProject.Admin.EntityFramework.SqlServer/"]
COPY ["src/SampleProject.Shared/SampleProject.Shared.csproj", "src/SampleProject.Shared/"]
COPY ["src/SampleProject.Admin.EntityFramework.PostgreSQL/SampleProject.Admin.EntityFramework.PostgreSQL.csproj", "src/SampleProject.Admin.EntityFramework.PostgreSQL/"]
COPY ["src/SampleProject.Admin.EntityFramework.MySql/SampleProject.Admin.EntityFramework.MySql.csproj", "src/SampleProject.Admin.EntityFramework.MySql/"]
RUN dotnet restore "src/SampleProject.Admin/SampleProject.Admin.csproj"
COPY . .
WORKDIR "/src/src/SampleProject.Admin"
RUN dotnet build "SampleProject.Admin.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "SampleProject.Admin.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENV ASPNETCORE_FORWARDEDHEADERS_ENABLED=true
ENTRYPOINT ["dotnet", "SampleProject.Admin.dll"]