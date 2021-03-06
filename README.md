VueBase is a Boilerplate for vuejs, a quick way to create administrative systems with login and password already built with jwt

# Technologies

* Backend in nodejs
* Frontend in javascript (Vuejs) and BootstrapVue

# Support

* Uploads with authenticated routes
* Https

# Features

* Generic components on frontend: Nav, Pagination, CrudHeader and AlertMessage
* Global Mixin
* Complete user and client CRUD (User with upload, we are using Multer on backend )
* Authentication using Vuex
* Http with axios
* VueRouter for route manage

# SETUP

```
sudo npm install -g nodemon
```

```
cd backend
cp config.js.dist config.js
npm install
nodemon index.js

cd ..
cd frontend
npm install
npm run serve
```

## Go to:

{project folder}/backend/app/produtor/

## then run 

node createRoot.js

### this is just to create the frst user, login: fundacao, pass: 123

## Access

http://localhost:8080

And thats all folks!
