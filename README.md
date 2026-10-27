1. Project structure
program-4/
├── index.js
├── package.json
├── package-lock.json
├── Dockerfile
└── .dockerignore


2. Create project and files
mkdir program-4
cd program-4
npm init -y
npm install express


File 1: index.js
nano index.js


const express = require("express");

const app = express();
const PORT = 3000;

app.get("/", (req, res) => {
    res.send("Hello from Git + Docker Integration!");
});

app.listen(PORT, "0.0.0.0", () => {
    console.log(`Server running on port ${PORT}`);
});


File 2: Dockerfile
nano Dockerfile


# Stage 1: Build Stage
FROM node:18 AS build

WORKDIR /app

COPY package*.json ./
RUN npm install

COPY . .

# Stage 2: Runtime Stage
FROM node:18-slim

WORKDIR /app

COPY --from=build /app /app

EXPOSE 3000

CMD ["node", "index.js"]


File 3: .dockerignore
nano .dockerignore


node_modules
.git
npm-debug.log


3. Build and run Docker
docker build -t prgm-4 .
docker images


Run the container:
docker run -d -p 3001:3000 --name program4-container prgm-4


Check the container:
docker ps


Test in the browser:
http://localhost:3001Expected output:
Hello from Git + Docker Integration!


4. Git commands and GitHub steps
Create an empty GitHub repository named program-4 at GitHub.

Run the following commands inside the project folder:
git init
git add .
git commit -m "Initial commit"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/program-4.git
git push -u origin main


Replace YOUR_USERNAME with your GitHub username.

For HTTPS authentication, use your GitHub Personal Access Token (PAT) as the password, not your normal GitHub password.
Update code and push changes
git add .
git commit -m "Updated application"
git push


5. Commands to resolve the errors we faced
Error 1: Port already allocated
docker ps
docker stop $(docker ps -q)


Run on another port if necessary:
docker run -d -p 3002:3000 --name program4-container prgm-4


Error 2: Container name already in use
docker rm -f program4-container


Then run:
docker run -d -p 3001:3000 --name program4-container prgm-4


Error 3: No running containers
docker ps -a


If docker stop $(docker ps -q) reports that an argument is required, there are no running containers to stop.
Error 4: Remove all stopped containers
docker container prune -f


Error 5: Rebuild after changing code
docker rm -f program4-container
docker build -t prgm-4 .
docker run -d -p 3001:3000 --name program4-container prgm-4


Error 6: Check application logs
docker logs program4-container


Error 7: Verify Docker and Git
docker images
docker ps -a
git status
git remote -v


6. Expected outcome
- Node.js application runs inside Docker.
- Docker image is built using a multi-stage Dockerfile.
- GitHub stores the source code and version history.
- Updated code can be committed, pushed, and rebuilt.
