# SET BASE IMAGE TO CREATE THE IMAGE FOR REACT APP
FROM node:20-alpine

#create a user with permission to run the app
#-S create a system user
#-G add the user to a group
#this is done to avoid running the app as root user
#if the app is run as root user, it can cause security issues
#it is a good practice to run the app as a non-root user
RUN addgroup app && adduser -S -G app app

#set the user to run the app
USER app

#set the working directory for the app
WORKDIR /app

#copy the package.json and package-lock.json files to the working directory
#this is done before copying the rest of the files to take advantage of Docker's caching mechanism
#if the package.json and package-lock.json files are not changed, Docker will use the cached layer and will not run npm install again
COPY package*.json ./

#sometimes the ownership of the files copied to the working directory is not set to the user running the app, which can cause permission issues when running npm install
#this command changes the ownership of the files to the user running the app
USER root
RUN chown -R app:app /app

#CHANGE BACK TO THE USER RUNNING THE APP
USER app

#install the dependencies
RUN npm install

#copy the rest of the files to the working directory
COPY . .

#expose the port on which the app will run
EXPOSE 5173

#command to run the app
CMD npm run dev
