
# Django and Docker Integration: A Developer's Guide

**What is Docker:** 
Docker is a platform and tool designed to make it easier to create, deploy, and run applications by using containers. Containers are lightweight and portable environments that package everything needed to run an application, including the code, runtime, libraries, and dependencies. Docker provides a consistent and isolated environment for applications, making it easier to develop, test, and deploy software across different environments.

## Table of Contents

- [Django and Docker Integration: A Developer's Guide](#django-and-docker-integration-a-developers-guide)
  - [Table of Contents](#table-of-contents)
  - [Create a Docker container for a Django application](#create-a-docker-container-for-a-django-application)
  - [Upload your Docker image to Docker Hub](#upload-your-docker-image-to-docker-hub)
  - [How to remove a Docker image](#how-to-remove-a-docker-image)

## Create a Docker container for a Django application

1. **Setup Django Project:**
   
   ```bash
   django-admin startproject projectname
   ```

   Replace `projectname` with the name of your project.

2. **Dockerfile:** 
   Create a Dockerfile in your project directory to define how your Docker image should be built. Here's a basic example of a Dockerfile for a Django project:

   ```Dockerfile
   # Use an official Python runtime as a parent image
   FROM python:3.8-slim-buster

   # Create and set the working directory
   WORKDIR /app

   # Copy the requirements file into the container
   COPY requirements.txt /app/

   # Install any needed packages specified in requirements.txt
   RUN pip install -r requirements.txt

   # Copy the current directory contents into the container at /app/
   COPY . /app/

   CMD [ "python3", "manage.py", "runserver", "0.0.0.0:8000"]
   ```


3. **requirements.txt:**
   Create a `requirements.txt` file in your project directory that lists all the Python packages your Django project depends on. You can generate this file using `pip freeze` if you already have your project set up:

   ```bash
   pip freeze > requirements.txt
   ```

4. **Build the Docker Image:**
   Open a terminal in your project directory and build the Docker image using the following command:

   ```bash
   docker build -t my-django-app .
   ```

   Replace `my-django-app` with your desired image name.

5. **Run the Docker Container:**
   After building the image, you can run a Docker container from it using the following command:

   ```bash
   docker run -it -p 8000:8000 my-django-app
   ```

   This command maps port 8000 from the container to port 8000 on your host machine. You can change the port if needed.

6. **Access the Django Application:**
   Once the container is running, you can access your Django application by opening a web browser and navigating to http://localhost:8000 (or the port you specified).

<br/>

## Upload your Docker image to Docker Hub

1. **Create a Docker Hub Account:**
   If you don't have a Docker Hub account, go to [Docker Hub](https://hub.docker.com/) and sign up for a free account.

2. **Log In to Docker Hub:**
   Open a terminal and log in to your Docker Hub account using the `docker login` command. Replace `<username>` with your Docker Hub username:

   ```bash
   docker login -u <username>
   ```

   You will be prompted to enter your Docker Hub password.

3. **Tag Your Docker Image:**
   Docker images need to be tagged with your Docker Hub username and the repository name. Use the `docker tag` command to do this:

   ```bash
   docker tag my-django-app <username>/my-django-app:tagname
   ```

   Replace `<username>` with your Docker Hub username, `my-django-app` with the name of your Docker image, and `tagname` with a version or tag name for your image (e.g., `v1`, `latest`, etc.).

4. **Push Your Docker Image:**
   To upload your Docker image to Docker Hub, use the `docker push` command:

   ```bash
   docker push <username>/my-django-app:tagname
   ```

   This command will upload the image to your Docker Hub account. Replace `<username>` with your Docker Hub username and use the same tag name you used when tagging the image.

5. **Verify on Docker Hub:**
   Go to your Docker Hub account in a web browser and log in. You should see your pushed Docker image in your repository.

Your Docker image is now available on Docker Hub and can be pulled and used by others. Make sure to keep your Docker Hub credentials secure, as they are used to upload and manage your images.

<br/>

## How to remove a Docker image

**Note:** If there is any image that is currently associated with a running or stopped container. Docker doesn't allow you to delete an image if there are containers that are based on it. To resolve this issue, you should follow these steps:

1. **List Running Containers:**
   First, list all the running containers to identify which one is using the image you want to delete. You can do this with the following command:

   ```bash
   docker ps
   ```

   Note down or copy the Container ID or Name.

2. **Stop and Remove the Container:**
   To stop and remove the container, use the `docker stop` and `docker rm` commands. Replace `<container_id_or_name>` with the actual Container ID or Name you found in the previous step:

   ```bash
   docker stop <container_id_or_name> # return id
   docker rm <container_id_or_name> # return id
   ```

   This will stop and remove the container, allowing you to delete the associated image.

3. **Delete the Docker Image:**
   After removing the container, you can delete the Docker image:

   ```bash
   docker rmi docker/<image-name>:tag
   ```

   Replace `docker/<image-name>:tag` with the name of the image you want to delete.

4. **Verify Image Deletion:**
   Verify that the image has been successfully deleted by listing your Docker images:

   ```bash
   docker images
   ```

   The image `docker/<image-name>:tag` should no longer appear in the list.
