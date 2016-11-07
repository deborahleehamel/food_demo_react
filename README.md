# Using create-react-app with a rails server

This project is a build from a tutorial at [FullStack React](git remote add origin git@github.com:deborahleehamel/food_demo_react.git)

Includes:

## Rails API

* built up the app myself wit rails new `food-demo --api`

* created food model in `app/models/food.rb`

* created foods controller in `app/controllers/foods_controller.rb`

* added new route scoped to `/api` and `foods#index`

* created migration to create table for foods with nutrient columns `bundle exec rake db:create db:migrate db:seed`

* rails server is at localhost:3001, provides single API endpoint,`/api/food`, and expects single parameter `q`, the food we are searching for

* tested endpoint in Postman `localhost:3001/api/food?q=hash+browns`

## React app

* run `create-react-app client` within the food-demo top directory

* see it in action by running `npm start` from inside of the client folder

* we have API server in top directory and client app inside client directory

* add `gem 'foreman', '~> 0.82.0'` to enable booting both servers with one command

* touch `Procfile` to declare two processes: one for `web` (our React app) and one for `api` (our Rails server):

  `web: cd client && npm start`

  `api: bundle exec rails s -p 3001`
* boot Foreman command with `foreman start -p 3000`

* to simplify, create Rake task to execute this task in `lib/tasks/start.rake`

  ```
   task :start do

    exec 'foreman start -p 3000'

  end
    ```
* React components live in `App.js`

* `Client.js` contains a Fetch call to our API endpoint and this is the one touchpoint between React web app and the API server:

  ```
  function search(query) {
    return fetch(`/api/food?q=${query}`, {
      accept: 'application/json',
  }).then(checkStatus)
      .then(parseJSON);
  }
  ```

* set up proxy for API server:
  ```
  // Inside client/package.json
  "proxy": "http://localhost:3001/",
```

* React app is ready and in place in `client/`

* Boot both servers with `rake start` to test it out
