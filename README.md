
# Contacts Vue APP

Front-end for the breakable toy contacts app.


## Authors

- [@gibranhl](https://twitter.com/gibranhl)

  
## Tech Stack

**Client:** VueJS, Vuetify, TailwindCSS
  
## Installation 
 1.Modify package.json file from start to serve
 ```json 
  "scripts": {
    "start": "vue-cli-service serve",
    "build": "vue-cli-service build",
    "lint": "vue-cli-service lint"
  },
 ```
 2.Install contacts-app with npm
```bash 
  npm install
  npm run serve
```
    
## Deployment

To deploy the app on heroku run

1.Modify package.json file from serve to start
 ```json 
  "scripts": {
    "serve": "vue-cli-service serve",
    "build": "vue-cli-service build",
    "lint": "vue-cli-service lint"
  },
 ```
2.Comit to your repo
 ```bash 
  git init
  git add .
  git commit -m "initial commit"
 ```
3.Build app
 ```bash 
  npm run build
 ```
4.Create and config on heroku
 ```bash 
  heroku login
  heroku create
  heroku buildpacks:add heroku/nodejs
  heroku buildpacks:add https://github.com/heroku/heroku-buildpack-static
 ```
3.Publish on heroku
```bash 
  git push heroku master
```
## Acknowledgements
Thank you for all the help and tips
- Dan Portillo
- Moy Morales


  