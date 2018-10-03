
#How to run new foxer360 project

## Prerequisities

[Git Large File Storage](https://git-lfs.github.com/)
[Docker](https://www.docker.com/)
[Docker compose](https://github.com/docker/compose)
[Prisma](https://www.prisma.io/)

## Steps
 1. Make new repository and clone [backoffice](https://github.com/Foxer-360/backoffice) and [server](https://github.com/Foxer-360/server) to it.

```
mkdir example-project
cd example-project/
git clone git@github.com:Foxer-360/server.git
git clone git@github.com:Foxer-360/frontend.git
```
2. Make `docker-compose.yml` in the project root folder.
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
3. Add `components.json`  and `plugins.json` to backoffice.	

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
4. Make .env file from .env.sample and fill it.
