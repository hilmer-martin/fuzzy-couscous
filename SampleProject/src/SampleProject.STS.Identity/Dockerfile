FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["src/SampleProject.STS.Identity/SampleProject.STS.Identity.csproj", "src/SampleProject.STS.Identity/"]
COPY ["src/SampleProject.Admin.EntityFramework.Shared/SampleProject.Admin.EntityFramework.Shared.csproj", "src/SampleProject.Admin.EntityFramework.Shared/"]
COPY ["src/SampleProject.Shared/SampleProject.Shared.csproj", "src/SampleProject.Shared/"]
RUN dotnet restore "src/SampleProject.STS.Identity/SampleProject.STS.Identity.csproj"
COPY . .
WORKDIR "/src/src/SampleProject.STS.Identity"
RUN dotnet build "SampleProject.STS.Identity.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "SampleProject.STS.Identity.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENV ASPNETCORE_FORWARDEDHEADERS_ENABLED=true
ENTRYPOINT ["dotnet", "SampleProject.STS.Identity.dll"]