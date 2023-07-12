## Containers + CI/CD

#### Task: Build a container image, and push it into a registry
### Scenario: You have a simple Python application written with Flask. You need to build a container image so that it can be deployed somewhere later on.
### To Do:
- Create a Hello World Python Flask application with an endpoint (you can copy-paste example code from Flask documentation)
- Prepare the Dockerfile, which can be used to build the container. Provide information how to build the image locally (with Docker/Podman) and run the container
- Prepare CI process (GitHub Workflows/Gitlab CI), which builds the container image, and pushes it to a registry (e.g. Docker Hub, Quay.io, GitHub Container Registry). -- The CI should be run on push to main branch, and periodically on Saturday at 7 PM (push an image with the latest tag)

## Let get started

#### Step 1: Create the Flask Application
- Create a new directory for the project and navigate into it. Then, create a new file called ***`app.py`*** and open it in a text editor. Add the following code to create a simple Flask application with an endpoint:

 ```
from flask import Flask

app = Flask(__name__)

@app.route('/')
def hello_world():
    return 'Hello, World!'

if __name__ == '__main__':
    app.run(host='0.0.0.0')
```

- Save the file.

#### Step 2: Create a Dockerfile 
- In the same directory as the ***`app.py`*** file, create a new file called Dockerfile (without any file extension) and open it in a text editor. Add the following content to the file:

```
FROM python:3.9-slim

WORKDIR /app

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY . .

EXPOSE 5000

CMD ["python", "app.py"]
```
- Save the file.

#### Step 3: Create a requirements.txt file
- Create a new file called ***`requirements.txt`*** in the project directory and add the following line to it:

```
flask
```
- Save the file.

#### Step 4: Build the Docker Image
- Open a terminal or command prompt and navigate to the project directory. Run the following command to build the Docker image:
  
<img width="1023" alt="a" src="https://github.com/OloruntobiOlurombi/containers-cicd/assets/40290711/3feafbde-0cf6-4091-a5fc-fca2fd1ba617">

```
docker build -t flask-app .
```
- This command builds the Docker image using the Dockerfile in the current directory and tags it as flask-app.

#### Step 5: Run the Docker Container 
- After the image has been built, run a container based on it. Use the following command to start the container:

![Screenshot 2023-07-10 at 11 30 12](https://github.com/OloruntobiOlurombi/containers-cicd/assets/40290711/23255c5d-3c9e-4cb8-9f10-b285885aa948)

```
docker run -p 5001:5000 flask-app
```
- This command starts the container and maps port 5000 from the container to port 5001 on the local machine.

- You should now be able to access the Flask application by visiting http://localhost:5001 in your web browser. It will display the "Hello, World!" message.

<img width="1135" alt="Screenshot 2023-07-10 at 11 30 25" src="https://github.com/OloruntobiOlurombi/containers-cicd/assets/40290711/a66ec31d-eac0-4658-8cb2-17a5e8bba8aa">

#### Step 6: Set up CI with GitHub Workflows 

- To set up CI with GitHub workflows, create a new file called .github/workflows/ci.yml in the project repository. Add the following content to it:

```
name: CI

on:
  push:
    branches:
      - main
  schedule:
    - cron: '0 19 * * SAT'

jobs:
  build-and-push:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v2

      - name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v1

      - name: Login to DockerHub
        uses: docker/login-action@v1
        with:
          username: ${{ secrets.DOCKERHUB_USERNAME }}
          password: ${{ secrets.DOCKERHUB_TOKEN }}

      - name: Build and push Docker image
        uses: docker/build-push-action@v2
        with:
          context: .
          push: true
          tags: ${{ secrets.DOCKERHUB_USERNAME }}/${{ secrets.DOCKERHUB_REPO }}:latest

```

- Make sure you have the DockerHub repository set up and accessible with your DockerHub account. If you haven't created a repository on DockerHub yet, please do so before proceeding.

- Ensure that you have the correct DockerHub Username, Token and Repository name set in the GitHub repository secrets. Open the GitHub repository and got to the ***"Settings"*** tab. then click on ***"Secrets"***. Make sure you have set the ***`DOCKERHUB_USERNAME`*** secret to your DockerHub username, the ***`DOCKERHUB_TOKEN`*** to your DockerHub token and the ***`DOCKERHUB_REPO`*** secret to your DockerHub repository name.
  
<img width="925" alt="Screenshot 2023-07-11 at 11 02 49" src="https://github.com/OloruntobiOlurombi/containers-cicd/assets/40290711/53be3830-62e1-4752-8559-4d7a4f30fb95">

- Commit and push the changes to the GitHub repository.

<img width="564" alt="Screenshot 2023-07-10 at 21 17 48" src="https://github.com/OloruntobiOlurombi/containers-cicd/assets/40290711/2e87bdb8-99cf-4697-b7e1-9b61de0ab13d">

- Confirm the status of the workflow jobs:

<img width="1359" alt="Screenshot 2023-07-11 at 11 01 51" src="https://github.com/OloruntobiOlurombi/containers-cicd/assets/40290711/d6ff9ca2-cc9b-4d71-a577-f6d33bef4729">




