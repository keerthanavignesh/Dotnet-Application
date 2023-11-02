FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build

WORKDIR /build

# Copy project source files
COPY src ./src

# Restore, build & publish
WORKDIR /build/src
RUN dotnet restore
RUN dotnet publish --no-restore --configuration Release

# =======================================================
# Stage 2 - Assemble runtime image from previous stage
# =======================================================

FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base

# Seems as good a place as any
WORKDIR /app

# Copy already published binaries (from build stage image)
COPY --from=build /build/src/bin/Release/net6.0/publish/ .

# Expose port 5000
EXPOSE 5000

# Run the ASP.NET Core app
ENTRYPOINT dotnet dotnet-demoapp.dll
