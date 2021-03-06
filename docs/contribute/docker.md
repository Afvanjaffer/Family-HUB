# Build Docker image for test

If you want to build your own docker image for test, follow these steps:

## Use docker-compose to build and run your own image

Use this process if you want to modify test source code and build
your image to run.

Edit `.env` to set `DB_HOST=mysql` (as `mysql` is the creative name of the MySQL container).

Then run:

```sh
docker-compose build
docker-compose up
```

## Use Docker directly to run with your own database

Use this process if you're a developer and want complete control over
your test container.

Edit `.env` again to set the `DB_*` variables to match your database. Then run:

```sh
docker build -t testhq/testhq .
docker run --env-file .env -p 80:80 testhq/testhq  # to run testHQ
# ...or...
docker run --env-file .env -it testhq/testhq sh    # to get a prompt
```

Note that uploaded files, like avatars, will disappear when you
restart the container. Map a volume to
`/var/www/test/storage/app/public` if you want that data to persist
between runs. See `docker-compose.yml` for examples.

## Other documents to read	

[Connecting to MySQL inside of a Docker container](/docs/installation/docker-mysql.md)
[Use mobile app with standalone server](/docs/installation/mobile.md)
