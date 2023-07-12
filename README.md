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
