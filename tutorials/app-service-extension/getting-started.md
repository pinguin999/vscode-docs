---
Order: 1
Area: appservicetools
TOCTitle: Getting started
PageTitle: Deployment to Azire App Service with Visual Studio Code
MetaDescription: Deployment to Azure App Services with Visual Studio Code for Node.js and Python apps
DateApproved: 10/03/2018
---
# Deploy to Azure App Service using Visual Studio Code

This tutorial walks you through deploying a Node.js or Python application to Azure using the [Azure App Service](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice) extension. With this extension, you're able to deploy to Azure on Linux in a matter of minutes.

## Prerequisites

To complete this tutorial you need an Azure account, Visual Studio Code with the App Service extension, a Node.js or Python environment, and an app that you'd like to deploy.

### Azure account

If you don't have an Azure account, [sign up now](https://azure.microsoft.com/free/?utm_source=campaign&utm_campaign=vscode-tutorial-docker-extension&mktingSource=vscode-tutorial-docker-extension) for a free 30-day account with $200 in Azure credits to try out any combination of services.

### Visual Studio Code, Docker, and language runtime

Install the following:

- [Visual Studio Code](https://code.visualstudio.com/).

- A suitable Node.js or Python environment:
  - [Node.js and npm](https://nodejs.org/en/download), or
  - Python and the Python extension as described on [Python Tutorial - Prerequisites](/docs/python/python-tutorial.md).

### Azure App Service extension

The **Azure App Service extension** helps you create, manage, and deploy Web Apps to Azure App Service on Linux.

> <a class="tutorial-install-extension-btn" href="vscode:extension/ms-azuretools.vscode-azureappservice">Install the Azure App Service extension</a>

For details, explore the [App Service extension tutorial](../app-service-extension/getting-started.md) visit the [vscode-azureappservice GitHub repository](https://github.com/Microsoft/vscode-azureappservice).

### Sign in to Azure

Once the extensions are installed, log into your Azure account by navigating to the **Azure: App Service** explorer, select **Sign in to Azure**, and follow the prompts.

![Sign in to Azure](../images/app-service-extension/sign-in.png)

### Troubleshooting

If you see the error **"Cannot find subscription with name [subscription ID]"**, this may be because you are behind a proxy and unable to reach the Azure API. Configure `HTTP_PROXY` and `HTTPS_PROXY` environment variables with your proxy information in your terminal using `export`.

```sh
export HTTPS_PROXY=https://username:password@proxy:8080
export HTTP_PROXY=http://username:password@proxy:8080
```

If setting the environment variables doesn't correct the issue, contact us by clicking the **I ran into an issue** button below.

### App code

If you don't already have an app you'd like to work with, use one of the following:

- Node.js: follow the [Node.js tutorial](/docs/nodejs/nodejs-tutorial.md).

- Python: use either of the following samples:

  - [python-sample-vscode-flask-tutorial](https://github.com/Microsoft/python-sample-vscode-flask-tutorial), which is the result of following the [Flask Tutorial](/docs/python/tutorial-flask.md).

  - [python-sample-vscode-django-tutorial](https://github.com/Microsoft/python-sample-vscode-django-tutorial), which is the result of following the [Django Tutorial](/docs/python/tutorial-django.md).

## Prerequisite check

Before continuing, verify the following:

1. Build and run your app code locally on your computer, installing any packages on which your app depends. For Python apps, also generate a `requirements.txt` file using `pip freeze` (which are included in the samples) so that those dependencies can be automatically installed when deploying to Azure App Service.

1. In VS Code, verify that you see the email account of your Azure around in the Status Bar and your subscription(s) in the **Azure: App Service** explorer.

    ![VS Code status bar showing Azure account](../images/app-service-extension/azure-account-status-bar.png)

    ![VS Code Azure App Service explorer showing subscriptions](../images/app-service-extension/azure-subscription-view.png)

----

<a class="tutorial-next-btn" href="/tutorials/app-service-extension/create-app">I've installed the prerequisites</a>
<a class="tutorial-feedback-btn" onclick="reportIssue('node-deployment-azureappservice', 'getting-started')" href="javascript:void(0)">I ran into an issue</a>
