A library to add simple database migration support to loopback projects.
Migrations that have been run will be stored in a table called 'Migrations'.
The library will read the loopback datasources.json files based on the NODE_ENV environment variable just like loopback does.
The usage is based on the node-db-migrate project.

<strong>NOTE: This does not currently work with the loopback in memory DB.</strong>

<h2>CLI Usage</h2>
```
loopback-db-migrate [up|down|create] [options]

Down migrations are run in reverse run order.

Options:
  --datasource specify database name (optional, default: db)
  --since specify date to run migrations from (options, default: run all migrations)
  --directory specify directory where migration scripts will live (options, default: '/server/migrations/'). A trailing slash is added at the end if not present 
```

<h2>Using the CLI directly</h2>
Run all new migrations that have not previously been run, using datasources.json and database 'db':
```javascript
./node_modules/loopback-db-migrate/loopback-db-migrate.js up
```

Run all new migrations since 01012014 that have not previously been run, using datasources.json and datasources.qa.json and database 'my_db_name':
```javascript
NODE_ENV=qa ./node_modules/loopback-db-migrate/loopback-db-migrate.js up --datasource my_db_name --since 01012014
```

Run all migrations living in the '/server/schema-migrations/' directory that have not previously been run, using datasource.json and database 'db'.
```javascript
./node_modules/loopback-db-migrate/loopback-db-migrate.js up --directory /server/schema-migrations/
```

<h2>Using the CLI with npm by updating your package.json</h2>
```javascript
"scripts": {
  "migrate-db-up": "loopback-db-migrate up --datasource some_db_name",
  "migrate-db-down": "loopback-db-migrate down --datasource some_db_name"
}

npm run-script migrate-db-up
npm run-script migrate-db-down

NODE_ENV=production npm run-script migrate-db-up
NODE_ENV=production npm run-script migrate-db-down
```

<h2>Example migrations</h2>
```javascript
module.exports = {
    up: function(dataSource, next) {
        dataSource.models.Users.create({ ... }, next);
    },
    down: function(dataSource, next) {
        dataSource.models.Users.destroy({ ... }, next);
    }
};
```
```javascript
/* executing raw sql */
module.exports = {
    up: function(dataSource, next) {
        dataSource.connector.query('CREATE TABLE `my_table` ...;', next);
    },
    down: function(dataSource, next) {
        dataSource.connector.query('DROP TABLE `my_table`;', next);
    }
};
```