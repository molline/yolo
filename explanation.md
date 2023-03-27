
  ## Choice of the base image on which to build each container.
  - The docker file uses node:16.17.0-bullseye-slim . This is beacuse it imlements a  slimmed-down version of the operating system, based on a modern Debian OS, with a stable and active Long Term Support version of Node.js.
   ## Dockerfile directives used in the creation and running of each container.
  ## Backend
  ```
  FROM  node:16.17.0-bullseye-slim - gets the source image 
  WORKDIR /backend - sets the working directory to /backend
  ADD package.json package.json - copies the contents of the package.json into a package.json file
  RUN npm install - installs the dependencies specified in the package.json
  COPY . /backend - copies context to the backend directory
  EXPOSE 5000 - Exposes the container on port 5000
  CMD ["npm","run" , "start"] - starts the backend service
  ```

  ## Client
  ```
    FROM  node:16.17.0-bullseye-slim - gets the source image 
    WORKDIR /client - sets the working directory to /backend
    COPY package.json package.json - copies the contents of the package.json into a package.json file
    RUN npm install - installs the dependencies specified in the package.json
    COPY . . - copies context to the current directory
    EXPOSE 3000 - Exposes the container on port 3000
    CMD npm start -starts the client service
  ```
  ##  Docker-compose Networking (Application port allocation and a bridge network implementation) where necessary.
    
  ##  Docker-compose volume definition and usage (where necessary).
  - On creation of the docker compose file there was need to add a database service (mongodb)that would enable the app access MONGODB database as this does not come with the containers by default. On creation of the service a volume was defined i.e
  ```
     volumes:
      - ./data:/data/db 
  ```
  - This  mounts the ./data directory from the host machine to the /data/db directory in the container, which allows MongoDB to persist data across container restarts.

  ## Git workflow used to achieve the task.
    1. Forking the repository from Vinge1718/yolo
    2. Cloning the repository to my local environment
    3. Dependency installation to have the app run locally both for the backend and client services.
    4. Creation of a Dockerfile in both the client and the backend directories to create images to be pushed to dockerhub
    5. Creation of a docker compose file to  run container orchestartion
    6. Editing the different file to optimize how the app works.

  ## Successful running of the applications and if not, debugging measures applied.
    The app runs successfuly as at now however some of the challenges  faced and debbuging measures applied
  ### Client:
    1. On running npm start on the client there was an error:
     `ERROR :0308010C: digital envelope routines :unsupported `
     - Cause - The app was using react-screpts version 3.4.1 which is < version 5 which is currently supported
     - Solution - `sudo npm uninstall react-scripts` ; which uninstall the current scripts
                `sudo npm install react-scripts` ; the reinstallation installs and upgaraded version in my case from version 3.4.3 to version 5.0.1  
    2. Once the server started
    `[eslint]    EACCESS:permission, mkdir 'home/molline/Documents/moringa/yolo/client/node_module/.cache`
    - Cause - missing .cache file
     
    - Solution - `mkdir -p node_modules/.cache` ;creates a .cache file with parent directories as needed
                `chmod -R 777 node_modules/.cache`; changes the directory permissions for all users to have read write execute permissions.
  ### Backend  
    1. Missing mongodb drivers blocking access to the local mongodb
  - Solution - `npm install mongodb --save` 

  ### Docker Compose
    1. Conatiners exit with code 0 
    - Solution - added `tty:true` to the backend and client definitions which enables the containers to stay running with a virtual terminal.
    2. `MongooseServerSelectionError: connect ECONNREFUSED 127.0.0.1:27017 `
     - Solution: Added a volume mount to the mongo db service and included a MONGO_URI environment variable to be used for connectionto the backend and client resulting in persistence and a connection success.
     - volume
  ```
    volumes:
        - ./data:/data/db
  ```
    - environment variable 
  ```
      environment:
        - MONGODB_URI=mongodb://mongodb:27017/yolomy
      
  ```

   ## Good practices such as Docker image tag naming standards for ease of identification of images and containers.
    1. Used semantic version MAJOR.MINOR.PATCH i.e molline/yolo-backend:1.0.0,molline/yolo-client:1.0.0
    2. Descriptive commit nessages to have proper track of the workflow example dockerfile for backend and client and docker compose
    3. Using a base image that is stable and has long term support 
