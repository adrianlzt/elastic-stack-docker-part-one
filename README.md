# Bug nginx-kibana

Si configuramos nginx como frontend de kibana, vemos que, en algunos casos,
las segundas peticiones que lanzamos no son contestadas.

Para reproducer el problema, podemos seguir los siguientes pasos:

1. Levantar docker-compose

```bash
docker-compose up
```

2. Loguearnos en kibana

3. Coger la cookie que se genera (ver alguna de las peticiones que lanza el navegador tras logearnos)

4. Lanzar dos veces esta petición (cambiando la cookie):

```bash
curl 'http://localhost:8080/internal/bsearch' \
  -H 'Content-Type: application/json' \
  -H 'Cookie: sid=PONER_LA_COOKIE' \
  -H 'elastic-api-version: 1' \
  -H 'kbn-version: 8.11.1' \
  --data-raw '{ "batch": [ { "request": { "params": { "index": "system-*" } } } ] }'
```

La primera petición funcionará correctamente, pero la segunda no, no contestará.
