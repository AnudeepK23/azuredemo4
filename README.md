# azuredemo3

# Docs for the Azure Web Apps Deploy action: https://github.com/azure/functions-action
# More GitHub Actions for Azure: https://github.com/Azure/actions

name: Build and deploy Java project to Azure Function App - DemoApp67

on:
  push:
    branches:
      - master
  workflow_dispatch:

env:
  AZURE_FUNCTIONAPP_NAME: DemoApp67 # set this to your function app name on Azure
  PACKAGE_DIRECTORY: '.' # set this to the directory which contains pom.xml file
  JAVA_VERSION: '8' # set this to the java version to use

jobs:
  build-and-deploy:
    runs-on: windows-latest
    steps:
      - name: 'Checkout GitHub Action'
        uses: actions/checkout@v2

      - name: Setup Java Sdk ${{ env.JAVA_VERSION }}
        uses: actions/setup-java@v1
        with:
          java-version: ${{ env.JAVA_VERSION }}

      - name: 'Restore Project Dependencies Using Mvn'
        shell: pwsh
        run: |
          pushd './${{ env.PACKAGE_DIRECTORY }}'
          mvn clean package
          popd
      - name: 'Run Azure Functions Action'
        uses: Azure/functions-action@v1
        id: fa
        with:
          app-name: 'DemoApp67'
          slot-name: 'production'
          publish-profile: ${{ secrets.AzureAppService_PublishProfile_c29e1cdfa84c499487f12f8489287e42 }}
          package: '${{ env.PACKAGE_DIRECTORY }}'
          respect-pom-xml: true
         
