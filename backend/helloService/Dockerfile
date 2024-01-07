FROM node:18
WORKDIR /
COPY . /
RUN npm install
ENV PORT=3001
CMD [ "node", "index.js", "3001" ]