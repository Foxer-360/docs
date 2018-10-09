# Foxer360 CMS Overview

Foxer360 is a complete solution for easy-to-use creating websites with multilanguage support.

## Prerequisites

**Knowledges**
- JavaScript/TypeScript
- React, Apollo, GraphQL
- Node.JS, NPM/Yarn
- Base terminal commands
- Git, Our Git workflow

**System packages**
- Git, Git LFS
- Some IDE (VS Code, SublimeText, PHP Storm, Web Storm, Atom, Vim, ...)
- Docker, Docker-Compose
- Node.JS, NPM, Yarn
- Prisma (Globally installed node package)
- GraphQL-CLI (Globally installed node package)
- Linklocal (Globally installed node package)

## Install

There are only three important packages: **BackOffice, Frontend, Server**. I recommended to create folder for a whole project, for example `foxer360`. There will be all files for this project include database. Very important thing is, that packages like **BackOffice** are using another packages like **Composer** or **Delta**, etc. These packages are defined in `package.json` and they are linked direcly to *Git Repositories*. This means, that if you needs to make some changes in these packages, than you need clone repositories of these packages and link them locally. More about how to do this in **Local development**.

```bash
cd ~
mkdir foxer360
```

Before we start clonnig our repositories, we need to run docker with Prisma. For that we need `docker-compose.yml` file which is [Here](../docker/docker-compose.yml). So download and copy this file into root directory of this project (`foxer360` which we create above). Fox example on Linux

```bash
cp ~/Downloads/docker-compose.yml ~/foxer360
```

Than run docker compose. If you are using docker for another things, probably you know how to control it. But if you using docker just for this project and you know nothing about docker, remove all containers and images before you start. This removing is included in bash below.

```bash
cd ~/foxer360
sudo docker rm $(sudo docker ps -qa)
sudo docker rmi $(sudo docker images -a)
sudo docker-compose up -d
```

Know we can clone all three repositories

```bash
cd ~/foxer360
git clone https://github.com/Foxer-360/server.git
git clone https://github.com/Foxer-360/backoffice.git
git clone https://github.com/Foxer-360/frontend.git
cd server
git checkout sandbox
yarn
cd ../backoffice
git checkout sandbox
yarn
cd ../frontend
git checkout sandbox
yarn
cd ..
```

Now we have all three repos clonned and on `sandbox` branch. Also with installed dependencies. Now we need to specify `.env` files. Each of these three packages has own `.env.sample` file, which you can use to create `.env` file and fill it with correct informations. There are some default configurations.

**For server**
```
# Define production or development
NODE_ENV=development
# NODE_ENV=production

# Common variables
SERVER_PORT=8000
SOCKET_PORT=8080
AUTH0_DOMAIN=nevim42.eu.auth0.com
AUTH0_AUDIENCE=http://localhost:8000/

# Prisma
PRISMA_ENDPOINT=http://localhost:4466
```

**For frontend**
```
# Application
SERVER_PORT=3001
SERVER_URL=http://localhost

# GraphQL Server
REACT_APP_GRAPHQL_SERVER=http://localhost:8000/graphql
```

**For backoffice**
```
# +-------------------------------------+
# |               COMMON                |
# +-------------------------------------+
REACT_APP_ENABLE_REDUX_DEVTOOL=false

# +-------------------------------------+
# |             SERVER API              |
# +-------------------------------------+
REACT_APP_API_URL=http://localhost:8000
REACT_APP_API_TIMEOUT=15000

# +-------------------------------------+
# |                AUTH0                |
# +-------------------------------------+
REACT_APP_AUTH0_CLIENT_ID=C3APVkj7pSphv9x7qLZ7ib1eeyPO5lOh
REACT_APP_AUTH0_CLIENT_DOMAIN=nevim42.eu.auth0.com
REACT_APP_AUTH0_REDIRECT=http://localhost:3000/callback
REACT_APP_AUTH0_AUDIENCE=http://localhost:8000/

# +-------------------------------------+
# |               SOCKETS               |
# +-------------------------------------+
REACT_APP_SOCKETS_URL=http://localhost
REACT_APP_SOCKETS_PORT=8080

# Media Library
REACT_APP_MEDIA_LIBRARY_SERVER=http://18.195.243.10:8090

# Setup GraphQL server
REACT_APP_GRAPHQL_SERVER=http://localhost:8000/graphql

# To preview pages
REACT_APP_FRONTEND_URL=http://localhost:3001
```

Now we have almost configured whole project. But what we also need is specify `components` and `plugins` which we want to use. We have our `components` and `plugins` for that purpose. In both, **BackOffice** and **Frontend** create two files, `components.json` and `plugins.json`.

```bash
cd ~/foxer360
cd backoffice
touch components.json
touch plugins.json
cd ../frontend
touch components.json
touch plugins.json
```

And now fill these files with content below

**components.json**
```JSON
{
	"base": {
		"git": "https://github.com/Foxer-360/components.git#sandbox",
		"style": "styles/main.css",
		"assets": "styles/assets"	
	}
}
```

**plugins.json**
```JSON
{
	"system": {
		"git": "https://github.com/Foxer-360/plugins.git#sandbox"
	}
}
```

And now, we have all configurations. To load these `components` and `plugins` from configuration files, we need to run `yarn deps` script, which will do all things for us.

```bash
cd ~/foxer360
cd backoffice
yarn deps
cd ../frontend
yarn deps
```

Now we have all what we need. It's time to run

**Prisma**
```bash
cd ~/foxer360
sudo docker-compose up
```

**Server**
```bash
cd ~/foxer360/server
prisma deploy
yarn start
```

**BackOffice**
```bash
cd ~/foxer360/backoffice
yarn start
```

**Frontend**
```bash
cd ~/foxer360/frontend
yarn start
```

## Local development
