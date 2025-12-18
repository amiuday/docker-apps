# Nginx app
✍️ Your FIRST real Dockerfile (Static Website)
Step 1: Create project directory
mkdir docker-html && cd docker-html

Step 2: Create HTML file
nano index.html

<!DOCTYPE html>
<html>
<head>
  <title>Docker Mastery</title>
</head>
<body>
  <h1>Hello Uday 👋</h1>
  <p>This site is running inside a Docker container!</p>
</body>
</html>
Save & exit.

Step 3: Create Dockerfile
nano Dockerfile

# 1️⃣ Base image
FROM nginx:alpine

# 2️⃣ Copy website files into container
COPY index.html /usr/share/nginx/html/index.html

# 3️⃣ Inform Docker that container listens on port 80
EXPOSE 80

# 4️⃣ Start nginx in foreground
CMD ["nginx", "-g", "daemon off;"]


🔍 Line-by-line Explanation (THOROUGH)
FROM nginx:alpine

Uses lightweight Linux + nginx

Saves huge image size

ALWAYS first line

COPY index.html ...

Copies file from EC2 → image

Happens at build time, not runtime

EXPOSE 80

Documentation for container port

Does NOT open ports (Docker run does)

CMD [...]

Runs when container starts

Only one CMD allowed

Can be overridden

🔨 Build the Image
docker build -t uday-html .


Watch the output → each step = one layer.

▶️ Run the Container
docker run -d -p 8080:80 --name html-site uday-html
🌍 Access from Browser
http://<EC2_PUBLIC_IP>:8080


🎉 If this loads → YOU WROTE A REAL DOCKERFILE

Make Sure you allowed 8080 in EC2 SG's.


🧪 Debug like a Pro
docker ps
docker logs html-site
docker exec -it html-site sh


Inside container:

ls /usr/share/nginx/html


🔄 Restart a running container
docker restart <container_name_or_id>

================================================================================================


🛠 Day 2 Hands-on Plan
1️⃣ Create Node app
mkdir docker-node && cd docker-node
npm init -y
npm install express


Create index.js:

const express = require("express");
const app = express();

app.get("/", (req, res) => {
  res.send("Hello from Node Docker 🚀");
});

app.listen(3000, () => {
  console.log("App running on port 3000");
});


2️⃣ Dockerfile (PROPER WAY)
FROM node:18-alpine

WORKDIR /app

COPY package*.json ./
RUN npm install

COPY . .

ENV NODE_ENV=production

EXPOSE 3000
CMD ["node", "index.js"]

Note: Allow 3000 port in EC2 SG's.

3️⃣ Build & Run
docker build -t node-app .
docker run -d -p 3000:3000 --name node-app node-app

Access:

http://<EC2_PUBLIC_IP>:3000
