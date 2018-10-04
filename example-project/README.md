
#How to run new foxer360 project

## Prerequisities installed

[Git Large File Storage](https://git-lfs.github.com/)
[Docker](https://www.docker.com/)
[Docker compose](https://github.com/docker/compose)
[Prisma](https://www.prisma.io/)

## Steps
 ### 1. Make new repository and clone [backoffice](https://github.com/Foxer-360/backoffice) and [server](https://github.com/Foxer-360/server) to it.

	```
	mkdir example-project
	cd example-project/
	git clone git@github.com:Foxer-360/server.git
	git clone git@github.com:Foxer-360/backoffice.git
	cd ..
	cd server
	npm install
	cd ..
	cd backoffice 
	npm install
	```
### 2. Make `docker-compose.yml` in the project root folder.
	```yaml
	version: '3'
	services:
		# Database Service. We use PostgreSQL from official postgres library
		database:
			image: postgres
			restart: always
			ports:
				- 5432:5432
			environment:
				POSTGRES_USER: postgres
				POSTGRES_PASSWORD: password
			volumes:
				# There you can map local folder for DB files
				- ./database:/var/lib/postgresql/data
		# Prisma Service from official prisma library
		prisma:
			image: prismagraphql/prisma:1.16
			restart: always
			ports:
				- 4466:4466
			environment:
				PRISMA_CONFIG: |
					port: 4466
					# uncomment the next line and provide the env var PRISMA_MANAGEMENT_API_SECRET=my-secret to activate cluster security
					# managementApiSecret: my-secret
					databases:
						default:
							connector: postgres
							host: database
							port: 5432
							user: postgres
							password: password
							migrations: true
	```
### 3. Add `components.json`  and `plugins.json` to backoffice.	

	`components.json`
	```
	{
	  "base": {
	    "path": "../components",
	    "style": "styles/main.css",
	    "assets": "styles/assets"
	  },
	  "kohinoor": {
	    "git": "https://github.com/Foxer-360/components#sandbox",
	    "style": "styles/main.css"
	  }
	}
	```

	`plugins.json`

	```
	{
	  "system": {
	    "git": "git@github.com:Foxer-360/plugins.git#sandbox"
	  }
	}
	```
### 4. Make .env file from .env.sample and fill it.

	Backoffice `.env`

	```yaml
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

	Server `.env`
	```yaml
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
	# Variables for production
	# Variables for development
	```
### 5. Orchestrate dockers with `docker-compose up -d` in directory where your docker-compose occur.
### 6. Link dependencies and plugins `yarn deps` command on backoffice and then `yarn`.
### 7. Go to `server/database` and run `prisma deploy`.

### 8. Start the backoffice with `yarn start` and server with `nodemon`
   ```
