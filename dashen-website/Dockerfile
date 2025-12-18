# 1️⃣ Base image
FROM nginx:alpine

# 2️⃣ Copy website files into container
COPY index.html /usr/share/nginx/html/index.html

# 3️⃣ Inform Docker that container listens on port 80
EXPOSE 80

# 4️⃣ Start nginx in foreground
CMD ["nginx", "-g", "daemon off;"]



