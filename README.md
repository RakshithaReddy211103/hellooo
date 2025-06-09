name: Build and Deploy to Azure Web App

on:
  push:
    branches:
      - main

jobs:
  build-and-deploy:
    runs-on: windows-latest

    steps:
    - name: Checkout code
      uses: actions/checkout@v4

    - name: Setup .NET
      uses: actions/setup-dotnet@v4
      with:
        dotnet-version: '8.0.x'

    - name: Restore dependencies
      run: dotnet restore MyWebApp.sln

    - name: Build
      run: dotnet build MyWebApp.sln --configuration Release

    - name: Publish
      run: dotnet publish MyWebApp.sln --configuration Release --output ./publish

    - name: Deploy to Azure Web App
      uses: azure/webapps-deploy@v3
      with:
        app-name: <YOUR_AZURE_WEBAPP_NAME>
        slot-name: production
        publish-profile: ${{ secrets.AZURE_WEBAPP_PUBLISH_PROFILE }}
        package: ./publish# hellooo
