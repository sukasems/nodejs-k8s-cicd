FROM node:12-alpine

LABEL   maintainer1="Sukasems Siwaporn [sukasems@generali.co.th]" \
        description="nodejs-k8s-cicd" \
        version="1.0" \
        node_version="10"

#ENV NODE_ENV="development" \
#    NODE_APP_PORT=3000 \
#    NODE_APP_MSSQL_USER="sa"\
#    NODE_APP_MSSQL_PASSWORD="gtuser@19" \
#    NODE_APP_MSSQL_SERVER="192.168.0.153" \
#    NODE_APP_MSSQL_DB="DMSalesDataMart" \
#    NODE_APP_MSSQL_PORT=49179

# Create app directory
WORKDIR /usr/src/app

# Install app dependencies
# A wildcard is used to ensure both package.json AND package-lock.json are copied
# where available (npm@5+)
COPY package*.json ./

#RUN npm install
# If you are building your code for production
#RUN npm install --only=production
RUN npm install

# Bundle app source
COPY . .

EXPOSE 5000
CMD [ "npm", "start" ]