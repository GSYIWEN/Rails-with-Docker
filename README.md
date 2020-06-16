# Step 1: Build the project
With those files in place, you can now generate the Rails skeleton app using docker-compose run:
    ```
    docker-compose run web rails new . --force --no-deps --database=postgresql
	```

    First, Compose builds the image for the web service using the Dockerfile. Then it runs rails new inside a new container, using that image. 

    Now that you’ve got a new Gemfile, you need to build the image again. (This, and changes to the Gemfile or the Dockerfile, should be the only times you’ll need to rebuild.) run:

    ```
    docker-compose build
    ```

# Step 2: Connect the database🔗
The app is now bootable, but you’re not quite there yet. By default, Rails expects a database to be running on localhost - so you need to point it at the db container instead. You also need to change the database and username to align with the defaults set by the postgres image.
Replace the contents of config/database.yml with the following:

```
default: &default
  adapter: postgresql
  encoding: unicode
  host: db
  username: postgres
  password: root
  pool: 5

development:
  <<: *default
  database: myapp_development


test:
  <<: *default
  database: myapp_test
```

# Step 3: Boot the app
You can now boot the app with docker-compose up:
```
docker-compose up
```

Finally, you need to create the database. In another terminal, run:
```
docker-compose run web rake db:create
```
