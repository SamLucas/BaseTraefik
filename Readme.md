# Traefik

Antes de qualquer devemos modificar a permissão dos certificados e o ```acme.json```:

```
chmod 644 certs/certs.crt
chmod 600 certs/certs.key
chmod acme.json
```

Geramos uma senha para autenticação no painel administrativo do traefik:

 ```
$ sudo apt-get install apache2-utils
$ htpasswd -nb admin secure_password
```
E adicionamos no arquivo ```Traefik.toml``` na linha ```users``` e criamos a rede no docker:

 ```
    address = ":8080"
    [entryPoints.dashboard.auth]
      [entryPoints.dashboard.auth.basic]
        users = ["admin:$apr1$AApUOrD.$GzCHmAV5APE67NoYiQbfs/"]
  [entryPoints.http]
    address = ":80"
 ```

```
$ sudo docker network create web
```

Também é necessário alterar o dominio no arquivo do ```Traefik.toml``` e do ```docker-compose.yml```:

```
  entryPoint = "http"

[docker]
domain = "masterserver.cf"
watch = true
```