## Backend Service

 - Backend consists of two services.

```
-- helloService
-- profileService
```

## Hello Service

```
cd helloService
```

- Create a .env file with the port number to host the application.

```
nano .env
```
```
PORT=$PORT
```

- Create a Dockerfile to create a docker image to containerize the helloService application.

```
nano Dockerfile
```
```
FROM node:18
WORKDIR /
COPY . /
RUN npm install
ENV PORT=3001
CMD [ "node", "index.js", "3001" ]
```

- Build the docker image.

```
docker build -t <your-image-name:tag> .
```

- Host the application on a container by passing the env variable.

```
docker run -it -e PORT=3001 -p 3001:3001 <your-docker-image-name:tag>
```
![Screenshot (120)](https://github.com/TeamKanyarasi/MERN_App_Microservices/assets/139607786/70fcd461-904f-419c-8539-62e608370b15)

-  Create a public repository and push the image to the ECR.

![Screenshot (123)](https://github.com/TeamKanyarasi/MERN_App_Microservices/assets/139607786/a8018e83-574a-4880-8557-14d968452581)

![Screenshot (124)](https://github.com/TeamKanyarasi/MERN_App_Microservices/assets/139607786/c03ec6dd-b356-41e9-9ff7-5bd9904ca92b)

## Profile Service

```
cd profileService
```

- Create a .env file with the port number to host the application and mongo url to establish a connection to the database.

```
nano .env
```
```
PORT=$PORT
MONGO_URL=$MONGO_URL
```

- Create a Dockerfile to create a docker image to containerize the profileService application.

```
nano Dockerfile
```
```
FROM node:18
WORKDIR /
COPY . /
RUN npm install
ENV PORT=3002
ENV MONGO_URL=MONGO_URL
CMD [ "node", "index.js", "3002" ]
```

- Build the docker image.

```
docker build -t <your-image-name:tag> .
```

- Host the application on a container by passing the env variable.

```
docker run -it -e PORT=3002 -e MONGO_URL="<your-mongodb-url>" -p 3002:3002 <your-docker-image-name:tag>
```

- Add users to the database.

![Screenshot (117)](https://github.com/TeamKanyarasi/MERN_App_Microservices/assets/139607786/f2a109d9-4a2c-4b5f-a1b7-91c06c01b82c)

- Fetch users from the database.

![Screenshot (118)](https://github.com/TeamKanyarasi/MERN_App_Microservices/assets/139607786/3e9d9363-bf50-4926-bb6e-0dfe725d6be0)

- DB results.

![Screenshot (119)](https://github.com/TeamKanyarasi/MERN_App_Microservices/assets/139607786/2ac35a46-7018-4e35-9e7b-ec44a241aa19)

-  Create a public repository and push the image to the ECR.

![Screenshot (125)](https://github.com/TeamKanyarasi/MERN_App_Microservices/assets/139607786/0f7b33be-ae25-4ddd-88e2-9a544916f525)

![Screenshot (126)](https://github.com/TeamKanyarasi/MERN_App_Microservices/assets/139607786/98f171a9-5427-4888-9e85-71fb118215cb)



