FROM mcr.microsoft.com/dotnet/core/sdk:3.0 AS build
WORKDIR /src

COPY PricingService.Api/*.csproj ./PricingService.Api/
COPY PricingService/*.csproj ./PricingService/
RUN  cd PricingService && dotnet restore

COPY PricingService.Api ./PricingService.Api/
COPY PricingService ./PricingService/
RUN cd PricingService && dotnet build

COPY PricingService.Test/*.csproj ./PricingService.Test/
RUN cd PricingService.Test && dotnet restore

COPY PricingService.Test ./PricingService.Test/
RUN cd PricingService.Test && dotnet build

FROM build AS test
WORKDIR /src/PricingService.Test
RUN dotnet test --verbosity:normal --no-restore

FROM build AS publish
WORKDIR /src/PricingService
RUN dotnet publish -c Release --no-restore -o out

FROM mcr.microsoft.com/dotnet/core/runtime:3.0 AS runtime
WORKDIR /app
EXPOSE 55269
EXPOSE 44392
COPY --from=publish /src/PricingService/out ./
ENTRYPOINT ["dotnet", "PricingService.dll"]
