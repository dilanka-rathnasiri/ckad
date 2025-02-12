# Docker

* Docker container final command: `ENTRYPOINT` + `CMD`
* Can override `CMD` in `docker run` command after adding required commands after the image name
* Can override `ENTRYPOINT` in `docker run` command with `--entrypoint`
* In kubernetes pod's manifest file:
    * `spec.containers[].command` ~ `ENTRYPOINT` of the Dockerfile
    * `spec.continaers[].args` ~ `CMD` of the Dockerfile

---

Example Dockerfile:

```Dockerfile
ENTRYPOINT ["python3", "app.py"]

CMD ["--color", "blue"]
```

## Command 1:

```shell
docker run test-image:latest
```

Final command 1:

```shell
python3 app.py --color blue
```

## Command 2:

```shell
docker run test-image:latest --color red
```

Final command 2:

```shell
python3 app.py --color red
```
