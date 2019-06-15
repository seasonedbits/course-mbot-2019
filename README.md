micro:bit course Season 1, powered by [remark](https://remarkjs.com/).

## Presentation

Serve this folder with [any HTTP server](https://gist.github.com/willurd/5720255), examples:

```sh
python3 -m http.server 8000
caddy -port 8000
docker run --rm -p 8000:80 -v "$PWD":/usr/share/nginx/html:ro nginx:alpine
```

[Presentation mode · gnab/remark Wiki](https://github.com/gnab/remark/wiki/Presentation-mode)

- `C`: clone presentation
- `P`: enter presentation mode

## Dev

```sh
npm i -g live-server
live-server
```

Modification of files in this folder will trigger browser to live reload.

## References

[Introduction · Bootstrap](https://getbootstrap.com/docs/4.1/getting-started/introduction/)
[Icons | Font Awesome](https://fontawesome.com/v5.0.13/icons?d=gallery&m=free)
