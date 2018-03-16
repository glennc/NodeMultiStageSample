FROM microsoft/dotnet:2.1-sdk as build
WORKDIR /src
COPY TestNukeNode.csproj .
RUN dotnet restore

FROM node:8 as clientbuild
COPY --from=build /src /src
WORKDIR /src/ClientApp/
COPY ./ClientApp/package.json .
COPY ./ClientApp/package-lock.json .
RUN npm install
COPY . ../
RUN npm run build -- --prod

FROM build as publish
COPY --from=clientbuild /src .
RUN dotnet publish -o /out -c Release /p:SkipWebPack=true

FROM microsoft/aspnetcore:2.1.0-preview1
WORKDIR /app
COPY --from=publish /out .
ENTRYPOINT ["dotnet", "TestNukeNode.dll"]
