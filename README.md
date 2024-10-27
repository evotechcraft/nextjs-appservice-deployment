# Next.js Deployment on Azure App Service

This sample project explains how to deploy a Next.js application to Azure App Service using GitHub Actions for continuous integration and deployment.

## Hire Us
Contact Email - contact@evotechcraft.com

## Free Support
For any questions or issues, please contact support at 
Support mail: support@evotechcraft.com

## Deploying the Next.js Application

Clone the this Repository and follow below steps.

## Github action file

```yml
# Docs for the Azure Web Apps Deploy action: https://github.com/Azure/webapps-deploy
# More GitHub Actions for Azure: https://github.com/Azure/actions

name: Build and deploy next.js app to Azure Web App - nextjs-appservice

on:
  push:
    branches:
      - main
  workflow_dispatch:

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - name: Set up Node.js version
        uses: actions/setup-node@v3
        with:
          node-version: '20.x'

      - name: npm install, build, and test
        run: |
          npm install
          npm run build


      - name: move static files
        run:  mv ./build/static ./build/standalone/build

      - name: Upload artifact for deployment job
        uses: actions/upload-artifact@v4
        with:
          name: app
          path: ./build/standalone

  deploy:
    runs-on: ubuntu-latest
    needs: build
    environment:
      name: 'Production'
      url: ${{ steps.deploy-to-webapp.outputs.webapp-url }}

    steps:
      - name: Download artifact from build job
        uses: actions/download-artifact@v4
        with:
          name: app

      - name: 'Deploy to Azure Web App'
        id: deploy-to-webapp
        uses: azure/webapps-deploy@v3
        with:
          app-name: 'nextjs-appservice-demo'
          slot-name: 'Production'
          publish-profile: ${{ secrets.AZUREAPPSERVICE_PUBLISHPROFILE_6918DA9FE89C484E8FE1DCE98899ECC4 }}
          package: .
```
Note: Replace `nextjs-appservice-demo` with your specific App Service name and ensure that the `AZUREAPPSERVICE_PUBLISHPROFILE` is set in your GitHub repository secrets.



## next.config.mjs

```ts
const nextConfig = {
    reactStrictMode: true,
    distDir: 'build',
    output: 'standalone',
    };
  
  
  export default nextConfig;
```

## Azure App service command
```
node server.js
```
![Azure App service command](/azure_configuration.png)


## License
This project is licensed under the MIT License. See the LICENSE file for more details.

## Issues

If you will face any issues with the usage of this package please raise one so that we can quickly fix it as soon as possible.

## Contributing

This is an open-source project under ```MIT License``` so anyone is welcome to contribute from typos, to source code to documentation.
