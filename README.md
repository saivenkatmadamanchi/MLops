# GitHub Lab
In this lab, we will navigate essential GitHub commands and explore the process of setting up Git actions.


### Create a GitHub Repository from the UI:

- Go to GitHub and create a new repository.

### Clone the Repository to the Local Machine:

```bash
git clone <repository_link>
```
### Check Current Working Branch:
```bash
git branch
```
### Create a New Feature Branch:
```bash
git branch <branch_name>
```
### Switch Between Branches:
```bash
git checkout <branch_name>
```

### Add Files to the Branch:
- To add all files:
```bash
git add .
```
- To add specific files:
```bash
git add <filename>
```
### Commit Changes:
```bash
git commit -m "<message>"
```
### Push Changes to the Branch:
```bash
git push
```

### Pull Latest Changes from GitHub:
```bash
git pull
```

### Merge Feature Branch to Master:
- Switch to the master branch:
```bash
git checkout master
```
- Merge the feature branch:
```bash
git merge <feature_branch_name>
```
- Commit the merge changes:
```bash
git commit -m "Merge <feature_branch_name> into master"
```
### Push the changes to GitHub:
```bash
git push
```



### GitHub Actions Workflow for Building and Pushing a Docker Image.

This GitHub Actions workflow is designed to automatically build and push a Docker image to Docker Hub when changes are pushed to the `master` branch or when pull requests are made to the `master` branch. This can be useful for automating the deployment of Dockerized applications.

### Workflow Configuration (application.yml)

The workflow is defined in the `.github/workflows/publish.yml` file. Here's a breakdown of its contents:

### Event Triggers

```yaml
on:
  push:
    branches: [ master ]
  pull_request:
    branches: [ master ]
```
This section defines the events that trigger the workflow. It runs whenever changes are pushed to the main branch or when pull requests are made to the main branch.

```yml
jobs:
  build:
```
Defines a job named "build" within the workflow.

```yml
  runs-on: ubuntu-latest
```
Specifies that the job should run on an Ubuntu-based runner. ubuntu-latest ensures the use of the latest available version of the Ubuntu runner.

```yml
steps:
    - uses: actions/checkout@v1
```
actions/checkout is used to check out the repository's code. This step ensures that the workflow has access to the codebase.

```yml
    - name: Docker Login
      env:
        DOCKER_USER: ${{ secrets.DOCKER_USER }}
        DOCKER_PASSWORD: ${{ secrets.DOCKER_PASSWORD }}
      run: |
        docker login -u $DOCKER_USER -p $DOCKER_PASSWORD
```

- This step is responsible for logging in to Docker Hub using the provided Docker credentials stored as GitHub secrets.
- DOCKER_USER and DOCKER_PASSWORD are environment variables set from secrets for secure Docker login.
- The docker login command authenticates the workflow to Docker Hub using the provided credentials.

```yml
    - name: Build and Push Image
      run: |
        docker image build -t owner_name/image_name:latest .
        docker push owner_name/image_name:latest
```
- In this step, the Docker image is built and pushed to Docker Hub.
- Docker image build is used to build the Docker image tagged as owner_name/image_name:latest. 
- The . specifies that the Dockerfile is in the root directory of the repository.
- Docker push is used to push the built image to Docker Hub.

### Usage
- Ensure that you have set up the required secrets (DOCKER_USER and DOCKER_PASSWORD) in the repository's GitHub settings. 
- These secrets should contain your Docker Hub credentials.
- Push changes to the main branch or create pull requests to the main branch to trigger the workflow.
- The workflow will automatically build and push the Docker image to Docker Hub on successful execution.
- This workflow streamlines the process of building and deploying Dockerized applications whenever changes are made to the main branch.
