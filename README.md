# mattermost-buildpack

> This buildpack aims at installing a [Mattermost](https://mattermost.com) instance on [Scalingo](https://www.scalingo.com) and let you configure it at your convenance.

[![Deploy to Scalingo](https://cdn.scalingo.com/deploy/button.svg)](https://my.scalingo.com/deploy?source=https://github.com/MTES-MCT/mattermost-scalingo#main)

## Usage

[Add this buildpack environment variable][1] to your Scalingo application to install the `Mattermost` server:

```shell
BUILDPACK_URL=https://github.com/MTES-MCT/mattermost-buildpack#main
```

## Configuration

You must have an add-on database `postgresql` or `mysql`.

Sample environment variables are set in a `.env.sample` file.

`PORT` and `DATABASE_URL` are provided by Scalingo.
`MATTERMOST_EDITION` and `MATTERMOST_VERSION` can be set by you in environment variables in Scalingo.
All other environment variables are specific to mattermost, see [documentation](https://docs.mattermost.com/administration/config-settings.html#environment-variables).

## Hacking 

You set environment variables in `.env`:

```shell
cp .env.sample .env
```

Run an interactive docker scalingo stack:

```shell
docker run --name mattermost -it -p 8065:8065 -v "$(pwd)"/.env:/env/.env -v "$(pwd)":/buildpack scalingo/scalingo-18:latest bash
```

And test in it:

```shell
bash buildpack/bin/detect
bash buildpack/bin/env.sh /env/.env /env
bash buildpack/bin/compile /build /cache /env
bash buildpack/bin/release
```

Run Mattermost server:

```shell
export PATH=$PATH:/build/mattermost/bin
mattermost server
```

You can also use docker-compose in order to test with a complete stack (db, s3, smtp):

```shell
docker-compose up --build -d
```

`.env.sample` is configured to work with this stack. You just need to create the bucket `mattermost` in minio.

[1]: https://doc.scalingo.com/platform/deployment/buildpacks/custom
