# Todo Laravel Vue App

> Laravel(version 7.15) backend API that uses the API resources with a Vue.js(version 2.6) frontend

## Quick Start

``` bash
# Install Dependencies
composer install

# Run Migrations
php artisan migrate

# Import Todos
php artisan db:seed

# Add virtual host if using Apache else localhost with port 8000 is default server

# Install JS Dependencies
npm install

# Watch Files
npm run watch
```

## Endpoints

### List all todos with links and meta
``` bash
GET api/todos
```
### Get single todo
``` bash
GET api/todo/{id}
```

### Delete todo
``` bash
DELETE api/todo/{id}
```

### Add todo
``` bash
POST api/todo
title
```

### Update todo
``` bash
PUT api/todo
todo_id/title
```


```

## App Info

### Author

Sanjay
sanjay.sorathiya@symmetrygroup.ie


