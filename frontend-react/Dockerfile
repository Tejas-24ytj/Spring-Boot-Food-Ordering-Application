# Use official Node.js image as the base image
FROM node:14 as build

# Set the working directory in the container
WORKDIR /app

# Copy the package.json and package-lock.json files into the container at /app
COPY package*.json ./

# Install dependencies
RUN npm install

# Copy the entire project directory into the container at /app
COPY . .

# Build the React app
RUN npm run build

# Use Nginx as the web server for serving React app
FROM nginx:alpine

# Copy the nginx configuration file
COPY nginx-ui.conf /etc/nginx/conf.d/default.conf

# Copy the built React app from the build stage to the nginx html directory
COPY --from=build /app/build /usr/share/nginx/html

# Expose port 80
EXPOSE 80

# Start Nginx server
CMD ["nginx", "-g", "daemon off;"]
