### Goal
I have an VueJs application created with vue-cli and want to integrate vue-cli-service with nginx reverse proxy. In order to call backend API and avoid CORS for development phase (with hot-reload)

nginx -> vue-cli-service -> backend

Assume backend API is served at `http://127.0.0.1:3000`
and vuejs app is served at `http://127.0.0.1:8080`

Nginx configuration for vue-cli-service development server

```
server {
  listen 81;
    server_name tanopwan.com;
    location /api {
      proxy_pass http://127.0.0.1:3000;
    }   
    location / {
      proxy_pass http://127.0.0.1:8080;
    }
    location /sockjs-node {
      proxy_pass http://127.0.0.1:8080;
      proxy_redirect off;

      proxy_http_version 1.1;
      proxy_set_header Upgrade $http_upgrade;
      proxy_set_header Connection "upgrade";
    }
  }
```

In package.json file
Make sure hostname that matches nginx server_name
```
"serve": "vue-cli-service serve --host tanopwan.com",
```

In hostfile /etc/hosts
```
127.0.0.1 tanopwan.com
```