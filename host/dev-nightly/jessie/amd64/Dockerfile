FROM microsoft/dotnet:2.0-sdk-jessie AS installer-env

ENV PublishWithAspNetCoreTargetManifest false

RUN apt-get update && \
    apt-get install git -y && \
    git --version && \
    git clone https://github.com/Azure/azure-webjobs-sdk-script.git && \
    cd azure-webjobs-sdk-script && \
    git fetch --all && \
    git checkout -b ahmels-management origin/ahmels-management && \
    dotnet build WebJobs.Script.sln && \
    dotnet publish src/WebJobs.Script.WebHost/WebJobs.Script.WebHost.csproj --output /azure-functions-runtime

# Runtime image
FROM microsoft/dotnet:2.0-runtime-jessie

COPY --from=installer-env ["/azure-functions-runtime", "/azure-functions-runtime"]

ENV AzureWebJobsScriptRoot=/home/site/wwwroot
ENV HOME=/home
ENV WEBSITE_HOSTNAME=localhost:80
ENV ASPNETCORE_URLS=http://+:80
EXPOSE 80

CMD ["dotnet", "/azure-functions-runtime/Microsoft.Azure.WebJobs.Script.WebHost.dll"]
