# Stage 1: Build React app
FROM node:18-alpine AS build-stage

WORKDIR /app

COPY package*.json ./
RUN npm install

COPY . .
RUN npm run build

# Stage 2: Serve với Nginx
FROM nginx:stable-alpine

# Copy file đã build vào thư mục mặc định của Nginx
COPY --from=build-stage /app/build /usr/share/nginx/html

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]