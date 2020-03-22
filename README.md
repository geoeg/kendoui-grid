# progress-kendoui-task

## Project setup
```
npm install
```

### Compiles and hot-reloads for development + json-server
```
npm run test
```
The command concatenates `json-server` and `npm run dev` scripts run in that order.
The second one runs a fake API using a simple .json file (the current example: ./db.json).
After running the script:
- the hot-reload vue app runs at [localhost:8080](localhost:8080) (*)
- the fake API runs at [localhost:3000](localhost:3000) (*). In the current example requests can be send at /products (for getting all the products from the API), or /products/n for getting a single product, where 'n' is a integer that equals the id of a product.

(*)The ports 8080 and 3000 are given fi they are not currently in use. If so - check the terminal for more information.

### Compiles and minifies for production
```
npm run build
```

### Lints and fixes files
```
npm run lint
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).
