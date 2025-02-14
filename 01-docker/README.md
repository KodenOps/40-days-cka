# **DOCKER**

## **How to Dockerize an Application and Manage Images Across Environments Using a Registry**

Consistency in the environment is crucial in software development. A common issue developers face is:

> *"It works on my system, but not in production!"*

To mitigate this, developers use **Docker** to create an image of the application. This image is then shared in a container registry, making it accessible for testing and production teams, ensuring consistency across environments.

---

## **Dockerfile**

A **Dockerfile** contains instructions to package an application and its dependencies into a Docker image. It automates the steps required to set up the environment.

### **Example: Dockerizing a React Application**

When setting up a React app manually, you would typically:
1. Install **Node.js** on your system.
2. Clone the project into a folder.
3. Open the project in a code editor.
4. Navigate to the project’s root directory via the terminal.
5. Run `npm install` to install dependencies.
6. Start the development server using `npm run dev`.

With **Docker**, these steps are automated and bundled into an image using a **Dockerfile**.

---

## **Writing a Dockerfile**

1. **Create a `Dockerfile` in the root of your project (without an extension).**  
2. **Specify a base image** (since we need Node.js for React):
   
   ```dockerfile
   FROM node:18-alpine
   ```
   - This pulls a lightweight version of Node.js.

3. **Set a working directory inside the container:**
   
   ```dockerfile
   WORKDIR /app
   ```
   - This defines where files will be stored inside the container.

4. **Copy project files into the container:**
   
   ```dockerfile
   COPY . .
   ```
   - This moves everything from the current directory into `/app` in the container.
   - Create a `.dockerignore` file to exclude unnecessary files (similar to `.gitignore`).

5. **Install dependencies:**
   
   ```dockerfile
   RUN npm install
   ```

6. **Define the command to start the application:**
   
   ```dockerfile
   CMD ["npm", "run", "dev"]
   ```

7. **Expose the required port (React apps usually run on port 3000):**
   
   ```dockerfile
   EXPOSE 3000
   ```

---

## **Final Dockerfile**

```dockerfile
# Use a lightweight Node.js base image
FROM node:18-alpine

# Set the working directory inside the container
WORKDIR /app

# Copy all project files into the container
COPY . .

# Install dependencies
RUN npm install

# Expose the port the app will run on
EXPOSE 3000

# Start the application
CMD ["npm", "run", "dev"]
```

---

## **Building the Docker Image**

Once the `Dockerfile` is ready, build the image using:

```sh
docker build -t my_image:latest .
```

- The `.` at the end means that the `Dockerfile` is in the current directory.
