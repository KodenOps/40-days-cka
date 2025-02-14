# DOCKER
## How to dockerize an application and manage the image across environment via registry
Consistency in the environment is paramount in software development. "It run on my system, it doesn't in production" is a quite popular scenerio in IT space. To mitigate this, the developer might have to imploy the use for a docker to create an image of the application. And this image is shared in an online registry for both test and production team to use. 

This way, all the requirements needed to run the application are bundled in layers as the image using Dockerfile.

### Dockerfile
This is a file where commands and instructions for bunding the application is written. This is a simple file that tried to mimick what handler of the code may have to do on their computer and at the same time add all the dependencies as a layer in the image.

Let's take for example, a typical React Application. When you clone the application, 
- You will first need to download node on your computer to be able to use npm package
- you will save it in a folder. 
- Then you will open it on a suitable code editor. 
- Then open terminal. Navigate to the root directory of the folder
- run `` npm install ``
- Then you run it in Dev/Production mode using `` npm run dev `` to start the application.

These processes are kinda automated and bundled as a single image using the Dockerfile

#### Syntax
Firstly, in the root of your application code, create a file and name it Dockerfile. No extension. 

- Depending on the type of application, you have to specify a base application. Since this is react app, normally, you would need to download node on your computer. So the starting point for our dockerfile will be node
```
FROM node:node:18-alpine

# this will pull a lighter version of node

```
- Next, you will need a folder for it to store and run. So, you will need to specify a WORKDIR (Work directory) where the docker will start working from. Note that the final stage of this dockerfile is to create an image which will be run to be a container. So, this WORKDIR is specifying the default directory for docker to place file. And by default, when you exec into the container, it will be in this folder.

```
FROM node:node:18-alpine
# this will pull a lighter version of node

WORKDIR /app
# this set default working directory to /app

```
- Since the Dockerfile is in the root directory, you will want to move all the files in the root files into the WORKDIR that you've created. Same way you will clone your application into a specific directory if you are doing it locally on your computer (outside docker). You can create `` .dockerignore `` file to specify files that should be ignored by docker. Same way `` .gitignore `` works.
```
FROM node:node:18-alpine
# this will pull a lighter version of node

WORKDIR /app
# this set default working directory to /app

COPY . .
# This copy everything in the directory where the Dockerfile is into the WORKDIR of the proposed container

```

- Now you need to run the `` npm install `` command to add the node modules

```
FROM node:node:18-alpine
# this will pull a lighter version of node

WORKDIR /app
# this set default working directory to /app

COPY . .
# This copy everything in the directory where the Dockerfile is into the WORKDIR of the proposed container

RUN npm install

```
- Now, that everything is set, you will want to run `` npm run dev `` to start the development environment. 

```
FROM node:node:18-alpine
# this will pull a lighter version of node

WORKDIR /app
# this set default working directory to /app

COPY . .
# This copy everything in the directory where the Dockerfile is into the WORKDIR of the proposed container

RUN npm install

CMD ["npm", "run", "dev"]

```
- One last thing, you want to ensure this runs on port 3000

```
FROM node:node:18-alpine
# this will pull a lighter version of node

WORKDIR /app
# this set default working directory to /app

COPY . .
# This copy everything in the directory where the Dockerfile is into the WORKDIR of the proposed container

RUN npm install

```
- Now, that everything is set, you will want to run `` npm run dev `` to start the development environment. 

```
FROM node:node:18-alpine
# this will pull a lighter version of node

WORKDIR /app
# this set default working directory to /app

COPY . .
# This copy everything in the directory where the Dockerfile is into the WORKDIR of the proposed container

RUN npm install

CMD ["npm", "run", "dev"]

EXPOSE 3000

```

This is complete now. And you can proceed to build the image of your application.

### Build Docker Image
After the completion of your Dockerfile, you can run the build command 
`` docker build -t <NAME FOR IMAGE>:<LABEL/TAG> </PATH WHERE DOCKERFILE IS> ``

E.g.
`` docker build -t my_image:latest . ``

The . means the Dockerfile is in the same directory you are currently.