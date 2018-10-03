---
Order: 3
Area: docker
TOCTitle: Add Docker files
PageTitle: Add Docker files to the app
MetaDescription: Add Docker files to the app that are necessary to create the Docker image
DateApproved: 10/03/2018
---
# Add Docker files

A container image is a bundle of your app code and its dependencies. To create an image, Docker needs a `Dockerfile` that describes how to structure the app code in the container and how to get that code running. The `Dockerfile`, in other words, is the template for your image. The Docker extension helps you create these files. If you don't already have app code for your image, refer to [Prerequisites - App code](getting-started.md#app-code) in the first step.

> **Note**: the Python samples linked earlier in this article already contain the necessary Docker files from this step.

1. in VS Code, open the **Command Palette** (`kb(workbench.action.showCommands)`) and select the **Docker: Add Docker files to workspace** command.

1. When the prompt appears after a few moments, select your app type, such as **Python** or **Node.js**.

1. Select the port on which your app listens. For the Node.js sample, use port 3000. For the Django sample, use 8000, and the Flask sample uses 5555.

1. With this information, the Docker extension creates the following files to describe your app and its environment:

    - The `Dockerfile` file describes the contents of your app's layer in the image, which are added in top of whatever base image you refer to. By default, the name of the image is the name of the workspace folder in VS Code.

    - A `.dockerignore` file that reduces image size by excluding files and folders that aren't needed in the image, such as `.git`, `.vscode`, and `node_modules`. For Python, add another line to the file for `__pycache__`.

    - `docker-compose.yml` and `docker-compose.debug.yml` files that are used with [Docker compose](https://docs.docker.com/compose/overview/). For the purposes of this tutorial, you can ignore or delete these files.

> **Tip:** VS Code provides great support for Docker files. See the [Working with Docker](/docs/azure/docker.md) topic to learn about rich language features like smart suggestions, completions, and error detection.

----

<a class="tutorial-next-btn" href="/tutorials/docker-extension/prepare-python-apps">I've created the Docker files for my app</a> <a class="tutorial-feedback-btn" onclick="reportIssue('docker-extension', 'add-docker-files')" href="javascript:void(0)">I ran into an issue</a>
