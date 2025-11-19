# set the base image to create the image for react app
FROM node:20-alpine

# create a user permissions to run the application
# -S -> create a system user
# -G -> specify the group for the user
# This is done to avoid running the application as root user inside the container
# If the app is run as root, it can lead to security vulnerabilities and potential exploits
# It's a best practice to run applications with the least privileges necessary
RUN addgroup app && adduser -S -G app app

# set the user to run the application
USER app

# set the working directory inside the container
WORKDIR /app

# copy the package.json and package-lock.json files to the working directory
# this is done before copying the entire application code to leverage Docker's layer caching mechanism
# If the dependencies in package.json have not changed, Docker can use the cached layer for faster builds   
COPY package*.json ./

# sometimes the ownership of the files in the working directory is changed to root
# and thus the app can't access files and throws an error -> EACCES: permission denied
# to avoid this, we change the ownership of the working directory to the app user
# RUN root

# change the ownership of the working directory to the app user
# chown -R -> change ownership recursively for all files and directories inside the working directory
# RUN chown -R app:app .

# change the user back to app user
USER app

# install the dependencies
RUN npm install

# copy the entire application code to the working directory
COPY . .

# expose port 5173 to the Docker that the container listens on the specified port at runtime
EXPOSE 5173

# start the application
CMD npm run dev