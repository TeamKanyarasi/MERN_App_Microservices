## Frontend Service

```
cd frontend
```

- Create a .env file with the urls of the backend services to establish a connection with the backend application.

```
nano .env
```
```
REACT_APP_HS_URL=$REACT_APP_HS_URL
REACT_APP_PS_URL=$REACT_APP_PS_URL
```

- Create a Dockerfile to create a docker image to containerize the frontend application.

```
nano Dockerfile
```
```
FROM node:18
WORKDIR /
COPY . /
ENV REACT_APP_HS_URL $REACT_APP_HS_URL
ENV REACT_APP_PS_URL $REACT_APP_PS_URL
RUN npm install
CMD [ "npm", "run", "start" ]
```

- Build the docker image.

```
docker build -t <your-image-name:tag> .
```

- Host the application on a container by passing the env variable.

```
docker run -it -e REACT_APP_HS_URL=http://<your-EC2-public-IP>:3001 -e REACT_APP_PS_URL=http://<your-EC2-public-IP>:3002/fetchUser -p 3000:3000 <your-docker-image-name:tag>
```

![Screenshot (121)](https://github.com/TeamKanyarasi/MERN_App_Microservices/assets/139607786/be302baf-f1aa-4748-b630-6190682cf96c)

- Create a public repository and push the image to the ECR.

![Screenshot (127)](https://github.com/TeamKanyarasi/MERN_App_Microservices/assets/139607786/37492c00-3de6-43f2-adc3-fda037eff8f7)

![Screenshot (128)](https://github.com/TeamKanyarasi/MERN_App_Microservices/assets/139607786/66d55450-8ac0-4c30-baad-65870bb9daa1)

- Containerized frontend and backend services.

![Screenshot (122)](https://github.com/TeamKanyarasi/MERN_App_Microservices/assets/139607786/6a1d5a09-156c-429c-bc39-53ae5577963b)

