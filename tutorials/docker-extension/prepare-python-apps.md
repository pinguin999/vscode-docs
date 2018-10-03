---
Order: 4
Area: docker
TOCTitle: Dockerfile changes for Python
PageTitle: Modify the Dockerfile for Python apps
MetaDescription: Make the necessary changes for Python apps to support container deployment to Azure App Services with Visual Studio Code
DateApproved: 10/03/2018
---

# Dockerfile modifications for Python apps

Node.js developers can skip this step; use the [button at the bottom](#next) to continue to the next step.

When creating Docker files for Python, the Docker extension by default uses the base image `python:alpine` and includes commands to run only the Flask development server. Such defaults don't accommodate Django, obviously, and when deploying to App Service you should use production-ready web servers instead of the development server. The following sections provide the specific details.

## Changes for Python/Flask apps

A good base image for Flask is tiangolo/uwsgi-nginx-flask:python3.6-alpine3.7, which is also available for other versions of Python (see the [tiangolo/uwsgi-nginx-flask respository](https://github.com/tiangolo/uwsgi-nginx-flask-docker) on GitHub). This image already contains Flask and the production-ready uwsgi and nginx servers.

By default, the image assumes that your app code is located in an `app` folder and that the Flask app object is named `app` and is found in `main.py`. Because your app, like the sample, may have a different structure, you can indicate the correct folders in the Dockerfile and provide the necessary parameters the the uwsgi server in a `uwsgi.ini` file.

The following steps summarize the configuration used in the [python-sample-vscode-flask-tutorial](https://github.com/Microsoft/python-sample-vscode-flask-tutorial) sample, which you can adapt to your own code.

1. The `Dockerfile` indicates the location and name of the Flask app object, the location of static files for nginx, and the location of the `uwsgi.ini` file. (The `Dockerfile` in the sample contains additional comments that are omitted here.)

    ```dockerfile
    FROM tiangolo/uwsgi-nginx-flask:python3.6-alpine3.7

    ENV LISTEN_PORT=5000
    EXPOSE 5000

    # Indicate where uwsgi.ini lives
    ENV UWSGI_INI uwsgi.ini

    # Tell nginx where static files live.
    ENV STATIC_URL /hello_app/static

    # Set the folder where uwsgi looks for the app
    WORKDIR /hello_app

    # Copy the app contents to the image
    COPY . /hello_app

    # If you have additional requirements beyond Flask (which is included in the
    # base image), generate a requirements.txt file with pip freeze and uncomment
    # the next three lines.
    #COPY requirements.txt /
    #RUN pip install --no-cache-dir -U pip
    #RUN pip install --no-cache-dir -r /requirements.txt
    ```

1. The `uwsgi.ini` file, which is in the `hello_app` folder of the sample, provides configuration arguments for the uwsgi server. For the sample, the configuration below says that the Flask app object is found in the `hello_app/webapp.py` module, and that it's named (that is, "callable" as) `app`. The other value are additional common uwsgi settings:

    ```ini
    [uwsgi]
    module = hello_app.webapp
    callable = app
    uid = 1000
    master = true
    threads = 2
    processes = 4
    ```

## Changes for Python/Django apps

A good base image for Flask is tiangolo/uwsgi-nginx:python3.6-alpine3.7, which is also available for other versions of Python (see the [tiangolo/uwsgi-nginx respository](https://github.com/tiangolo/uwsgi-nginx-docker) on GitHub).

This image already contains the production-ready uwsgi and nginx servers, but does not include Django. It's also necessary to provide settings to uwsgi so it can find the app's startup code.

The following steps summarize the configuration used in the [python-sample-vscode-django-tutorial](https://github.com/Microsoft/python-sample-vscode-django-tutorial) sample that you can adapt to your own code.

1. Make sure you have a `requirements.txt` file in your project that contains Django and its dependencies. You can generate `requirements.txt` using the `pip freeze` command.

1. In your Django project's `settings.py` file, add the root URL to which you intend to deploy the app to the `ALLOWED_HOSTS` list. For example, the following code assumes deployment to an Azure App Service (azurewebsites.net) named "vsdocs-django-sample-container":

    ```python
    ALLOWED_HOSTS = [
        # Example host name only; customize to your specific host
        "vsdocs-django-sample-container.azurewebsites.net"
    ]
    ```

    Without this entry, you'll eventually get all the way through the deployment only to see a "DiallowedHost" message that instructs to you add the domain to `ALLOWED_HOSTS`, which requires that you rebuild, push, and redeploy the image all over again!

1. Create a `uwsgi.ini` file in the Django project folder (alongside `manage.py`) that contains startup arguments for the uwsgi server. In the sample, the Django project is in a folder called `web_project`, which is where the `wsgi.py` and `setting.py` files live.

    ```ini
    [uwsgi]
    chdir = .
    module = web_project.wsgi:application
    env = DJANGO_SETTINGS_MODULE=web_project.settings
    uid = 1000
    master = true
    threads = 2
    processes = 4
    ```

1. Modify the `Dockerfile` to indicate the location of `uwsgi.ini`, set the location of static files for nginx, and make sure the SQLite database file is writable. (The `Dockerfile` in the sample contains additional comments that are omitted here.)

    ```dockerfile
    FROM tiangolo/uwsgi-nginx:python3.6-alpine3.7

    ENV LISTEN_PORT=8000
    EXPOSE 8000

    # Indicate where uwsgi.ini lives
    ENV UWSGI_INI uwsgi.ini

    # Tell nginx where static files live (as typically collected using Django's
    # collectstatic command.
    ENV STATIC_URL /app/static_collected

    # Copy the app files to a folder and run it from there
    WORKDIR /app
    ADD . /app

    # Make app folder writeable for the sake of db.sqlite3, and make that file also writeable.
    RUN chmod g+w /app
    RUN chmod g+w /app/db.sqlite3

    # Make sure dependencies are installed
    RUN python3 -m pip install -r requirements.txt
    ```

> **Note**: When building a Docker image on Windows, you typically see the message below, which is why the Dockerfile shown here includes the two `chmod` commands. If need to make other files writable, add the appropriate `chmod` commands to your Dockerfile.
>
> ```output
> SECURITY WARNING: You are building a Docker image from Windows against a non-Windows Docker host. All files and directories added to build context will have '-rwxr-xr-x' permissions. It is recommended to double check and reset permissions for sensitive files and directories.
> ```

----

<a class="tutorial-next-btn" name="next" href="/tutorials/docker-extension/containerize-app">I've made the necessary changes to my Dockerfile</a> <a class="tutorial-feedback-btn" onclick="reportIssue('docker-extension', 'prepare-python-apps')" href="javascript:void(0)">I ran into an issue</a>
