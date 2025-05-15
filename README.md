# mi_vue


```
docker run -itd --name mi_vue -v /home/rgonzalez/provis/vue/app:/app -p 5173:5173 -u $(id -u):$(id -g)  node:23-alpine
```
                                                                                                             
```
docker stop mi_vue && docker rm mi_vue
```

```
docker exec -it --user node mi_vue /bin/sh
```
