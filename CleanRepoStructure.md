# Clean Folder Structure

Based on my observations and learing from working on more than 5+ REST APIs projects in production I found the following structure of folder pretty neat both by code and files (Right now specific to **Node.js** applications)

```
src
├── config
│ ├── db.config.js
│ ├── logger.config.js
├── controllers
│ ├── Todos.controller.js
│ ├── Users.controller.js
│ ├── index.js
├── db
│ ├── redis.js
│ ├── mysql.js
│ ├── mongoose.js
│ ├── index.js
├── models
│ ├── Todo.model.js
│ ├── User.model.js
│ ├── index.js
├── routes
│ ├── App.routes.js
│ ├── Users.routes.js
│ ├── Todos.routes.js
│ ├── index.js
├── services
│ ├── Todos.service.js
│ ├── Users.service.js
│ ├── index.js
├── tests
│ ├── Todos.tests.js
│ ├── Users.tests.js
├── utils
│ └── Logger.js
│ ├── index.js
└── ExtraFolders/if you need
└── index.js
└── .env
```

## Description

- `src` contains all the code files
- `src/index.js` is the enrty point for the application
- `config` contains the different configurations that you may need to boot up your application
- `controllers` contains the business logic of the routes
- `db` contains the connection files to different databases/data persistance you choosen
- `models` contains your data models for `mongoose/sequelize`
- `routes` contains the api endpoints and use `controllers` for the logic
- `services` contains the code that interacts with your databases/persistance store and are used inside `controllers`
- `tests` contains your unit tests for the logic. Polite reminder: Unit tests are important
- `utils` contains all the code blocks as functions which are used more than once in your whole codebase.

## Example Repos

- **[Gaurav](https://github.com/igauravsehrawat)** has done a good job on implementing the above conventions in his [payroll system](https://github.com/igauravsehrawat/payroll-system)
- **RecastAI** also got something similar. Hava a look at their [bot-connector](https://github.com/RecastAI/bot-connector/) and also they got a [Wiki](https://github.com/RecastAI/bot-connector/wiki/00-Architecture)

## Credits

- The idea of having services was coined to me by **[Sudheer Signh Paliwal](https://github.com/justsudheerpaliwal)**, fantastic engineer and singer, all rounder in short. :blush:

### Todos

- Addition of code examples for each file in different folders
