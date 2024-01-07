FROM node:18
WORKDIR /
COPY . /
RUN npm install
ENV PORT=3002
ENV MONGO_URL=MONGO_URL
CMD [ "node", "index.js", "3002" ]