# Individual project

## Table of contents
- [Web Application](#web-application)
  - [Project description](#project-description)
  - [C4 Model](#c4-model)
    - [Context model](#context-model)
    - [Container model](#container-model)
    - [Component model](#component-model)
  - [Front-end project](#frontend-project)
  - [Back-end project](#backend-project)
- [Software quality](#software-quality)
  - [Security](#security)
  - [Testing](#testing)
  - [Code quality](#code-quality)
- [Continuous integration and deployment](#continuous-integration-and-deployment)
  - [Implementation](#implementation)
    - [Development pipeline](#development-pipeline)
    - [Production pipeline](#production-pipeline)
- [Professional](#professional)

## Web application
The goal of this learning outcome is to design and build a full-stack web application using a JavaScript framework in the front-end and an Object Oriented framework (.NET or Java) in the back-end. This application should use a distributed software architecture, Object Related Mapping and relevant communication protocols.

### Project description
For my individual project, I decided to make a service that resembles the now-defunct Nintendo Wi-Fi Connection for the Nintendo DS. This project consists of a collection of servers that are used by the Nintendo DS for online play, as well as a web-based front-end where the user can view various statistics and specify preferences for their Nintendo DS games.

### C4 Model

#### Context model
As displayed in the image underneath, the system in it’s entirety consists of a central service and a collection of game-specific services that the central service redirects the Nintendo DS systems to. Both the user and the Nintendo DS are expected to interact with the central service: the former by using the web-based front-end and the latter by connecting to the gateway. Only the central service and it’s accompanied databases are part of the scope of this project.

 ![Context model](https://i.imgur.com/gufo68C.png)

#### Container model
Zooming in on the project scope reveals that the project scope consists of a web-based front-end written in a JavaScript framework, a collection of web API’s and the databases that accompany these API’s. The front-end also communicates with an outside authentication system to validate the user’s identity.

![Container model](https://i.imgur.com/H6rQjRM.png)

#### Component model
Zooming in on the project once more shows that the central service consists of a number of API’s; each being it’s own microservice. All of these API’s are controlled by the endpoint; the gateway responsible for routing incoming requests to the appropriate API.

![Component model](https://i.imgur.com/n1G7kWj.png) 

### Frontend project
[TBA]

### Backend project
The back-end project consists of various microservices that are indirectly accessible through a gateway. All of the microservices use the .NET Core framework and thus are written in C#. Each microservice consists of either the first two or all three of the following layers:
  - The API layer, responsible for listening for, decyphering and responding to incoming requests.
  - The service layer, containing all microservice-specific logic.
  - The repository layer, responsible for making database calls using the ORM.

In order to allow individual layers to freely be replaced/changed, abstraction has been implemented using the factory method. Database calls are made using Entity Framework as the ORM, but is interchangeable because of the implementation of the repository pattern.

## Software quality
The goal of this learning outcome is to use a variety of tools to monitor the quality. This includes, testing, static code analisys and security.

### Security
[TBA]

Two research documents, each about a topic related to the security of the Nintendo DS, can be found [here](https://github.com/Blurrito/S3-Portfolio/blob/main/Research/README.md).

### Testing
In order to validate the code's functionality and ability to handle unexpected scenarios, the code needs to be thorougly tested. For the back-end, I decided to make use of the Xunit testing framework as it's easy to set up and allows for easy input data manipulation to minimize the amount of test logic that needs to be rewritten. There are two types of tests I created: unit tests and integration tests. The unit tests are meant to be small and only test a single piece of functionality, whereas the integration tests are meant for testing an entire sequence of actions.

![Test results](https://i.imgur.com/dv2BVM6.png)

### Code quality
In order to verify the quality of the written code, SonarCloud is used. This service part of the CI/CD pipeline, and is triggered with every push to the development branch. SonarCloud builds, tests and scans the code for vunerabilities, bugs, code smells and dublicated code. It then creates a detailed report consisting of all issues found, their locations, as well as possible solutions, which can then be viewed on the SonarCloud website. This information can then be used to improve the code quality. On top of this, it also runs the tests written for the project and determines the percentage of code blocks that these tests cover.

![SonarCloud scan results](https://i.imgur.com/jcoiMxs.png)

## Continuous integration and deployment
The goal of this learning outcome is to design and implement a CI/CD pipeline that is speficically made for this project capable of performing both integration and deployment. For the sake of this semester, deployment to Docker Hub is sufficient.

### Implementation
In order to automate the release process of my application, I implemented two CI/CD pipeline in my projects: a development pipeline and a production pipeline. As I made my code available in GitHub, this has been done with GitHub Actions. In each of my projects, I created two environments: Development and Production. These environments contain security measures such as environment secrets and security measures that will apply to each pipeline.

![Environments](https://i.imgur.com/HJpaf63.png)

#### Development pipeline
The development pipeline is set to trigger when a push is made to the development branch. The first job of the pipeline is to build and test the code. This is done by checking out the development branch and setting up the build environment. A machine with Ubuntu is used for this job because of the large difference in speed between Ubuntu and Windows.
```yaml
name: Development CI/CD

on:
  push:
    branches: development

jobs:
  build:
    runs-on: ubuntu-18.04
    steps:
      - name: Checkout
        uses: actions/checkout@v3

      - name: Setup build environment
        uses: actions/setup-dotnet@v2
        with:
          dotnet-version: '6.0.x'
```
 
Once the build environment has been set up, the pipeline will start with restoring the NuGet packages of the project, after which it will build the application. If this is successful, the pipeline will start running the unit tests, specifying the no-build flag to use the build made earlier in the pipeline.
```yaml
      - name: Restore NuGet packages
        run: dotnet restore ./Thims.Api/Thims.Api.sln

      - name: Build application
        run: dotnet build ./Thims.Api/Thims.Api.sln -c Release --no-restore
        
      - name: Run unit tests
        run: dotnet test ./Thims.Api/Thims.Api.sln -c Release --no-build
```

If the build job finishes successfully, the pipeline will then move on to the sonarcloud job. This job creates a code quality report by scanning, building and testing the code, and uploading the results to the SonarCloud website. This job makes use of the development environment for the SONAR_TOKEN secret, which is used by SonarCloud for authentication. Unlike the build job, this job runs on a Windows machine, because the .NET implementation of SonarCloud requires the use of PowerShell. The job starts with setting up the build environment. This process is identical to that of the build job, apart from the additional JDK setup for SonarCloud.
```yaml
  sonarcloud:
    runs-on: windows-latest
    environment: Development
    needs: build
    steps:
      - name: Checkout
        uses: actions/checkout@v3
        
      - name: Set up JDK 11
        uses: actions/setup-java@v1
        with:
          java-version: 1.11

      - name: Setup build environment
        uses: actions/setup-dotnet@v2
        with:
          dotnet-version: '6.0.x'
```

After the build environment has been set up, the pipeline checks for any cached instances of SonarCloud are present, and download the SonarCloud packages if this is not the case.
```yaml
      - name: Cache SonarCloud packages
        uses: actions/cache@v1
        with:
          path: ~\sonar\cache
          key: ${{ runner.os }}-sonar
          restore-keys: ${{ runner.os }}-sonar
        
      - name: Cache SonarCloud scanner
        id: cache-sonar-scanner
        uses: actions/cache@v1
        with:
          path: .\.sonar\scanner
          key: ${{ runner.os }}-sonar-scanner
          restore-keys: ${{ runner.os }}-sonar-scanner
        
      - name: Install SonarCloud scanner
        if: steps.cache-sonar-scanner.outputs.cache-hit != 'true'
        shell: powershell
        run: |
          New-Item -Path .\.sonar\scanner -ItemType Directory
          dotnet tool update dotnet-sonarscanner --tool-path .\.sonar\scanner
```

Finally, the pipeline starts executing the SonarCloud scan, uploading the results to the SonarCloud website so they can be inspected by the developer.
```yaml
      - name: Build and analyze
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}  # Needed to get PR information, if any
          SONAR_TOKEN: ${{ secrets.SONAR_TOKEN }}
        shell: powershell
        run: |
          .\.sonar\scanner\dotnet-sonarscanner begin /k:"Blurrito_Thims-API" /o:"blurrito" /d:sonar.login="${{ secrets.SONAR_TOKEN }}" /d:sonar.host.url="https://sonarcloud.io"
          dotnet build ./Thims.Api/Thims.Api.sln -c Release
          .\.sonar\scanner\dotnet-sonarscanner end /d:sonar.login="${{ secrets.SONAR_TOKEN }}"
```

#### Production pipeline
The production pipeline is set to trigger when a push to the main branch is made. The first job, the build job, is nearly identical to the build job in the development pipeline. The difference being that, at the end of the process, the successful build is uploaded as an artifact.
```yaml
      - name: Upload build as artifact for publishing
        uses: actions/upload-artifact@v3
        with:
          name: build
          path: ${{ env.DOTNET_ROOT }}/publish
```

After the build job has completed successfully, the pipeline continues with the publish job. This job makes use of the production environment, which requires manual approval before continuing as a security measure. The environment contains a list with reviewers that are allowed to approve or deny the request. Once the request has been approved, the pipeline will continue execution.
```yaml
  publish:
    environment: Production
    runs-on: ubuntu-18.04
    needs: build
```
![Deployment review](https://i.imgur.com/AJcFhbL.png)

After the deployment request has been approved, the pipeline will continue with retrieving the artifact that has been uploaded by the build job, which contains the successful build of the app.
```yaml
    steps:
      - name: Download build artifact
        uses: actions/download-artifact@v3
        with:
          name: build
```
 
The method of publishing the build varies widely depending on the destination. For the individual project, I decided to simply upload the image to my Docker Hub. In order to accomplish this, the machine first needs to login using my Docker Hub login details, which are stored in the production environment as secrets. After a successfull login attempt, the metadata of the Docker Hub repository gets extracted. Finally, the Docker image gets built and uploaded to the Docker Hub repository.
```yaml
      - name: Log in to Docker Hub
        uses: docker/login-action@f054a8b539a109f9f41c372932f1ae047eff08c9
        with:
          username: ${{ secrets.DOCKER_USERNAME }}
          password: ${{ secrets.DOCKER_PASSWORD }}
      
      - name: Extract metadata (tags, labels) for Docker
        id: meta
        uses: docker/metadata-action@98669ae865ea3cffbcbaa878cf57c20bbf1c6c38
        with:
          images: blurrito/centralservice.authentication
      
      - name: Build and push Docker image
        uses: docker/build-push-action@ad44023a93711e3deb337508980b4b5e9bcdc5dc
        with:
          context: .
          push: true
          tags: ${{ steps.meta.outputs.tags }}
          labels: ${{ steps.meta.outputs.labels }}
```
 
Alternatively, in my group project, I configured the pipeline to use an FTP client to upload the successful build to the .NET hosting server of Fontys. In order to prevent any dublicate file issues, the FTP client has been configured to first clear the destination directory.
```yaml
      - name: Deploy to server using FTP
        uses: SamKirkland/FTP-Deploy-Action@4.3.0
        with:
          server: ${{ secrets.SERVER_URL }}
          username: ${{ secrets.USERNAME }}
          password: ${{ secrets.PASSWORD }}
          local-dir: ./
          server-dir: ${{ secrets.APP_DIR }}
          protocol: ftps
          security: loose
          dangerous-clean-slate: true
```

## Professional
The goal of this learning outcome is to work in a professional manner. This includes, but is not limited to, working using an agile method and asking for, writing down and processing feedback from stakeholders.

### Feedback
Throughout the semester, I had meetings with the stakeholder(s) at various occasions to show my progress, as well as obtain feedback. After these conversations, I would convert my notes into a coherent story containing as many feedback points as possible and put this in FeedPulse so the stakeholders would be able to view and rate this story.

![FeedPulse](https://i.imgur.com/KEOyD4b.png)
