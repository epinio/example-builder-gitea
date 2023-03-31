# example-builder-gitea

## prerequisites

- `docker`
- `skopeo`
- `pack`



## Stack

Build the `bionic-full-stack`

```
./stacks/bionic-full-stack/scripts/create.s
```

This will create two OCI images in the `stacks/bionic-full-stack/build` folder:
- build.oci
- run.oci

```
cd stacks/bionic-full-stack/build
mkdir build && tar -xf build.oci -C build
mkdir run   && tar -xf run.oci   -C run
```

```
skopeo -v copy oci:build docker-daemon:ghcr.io/enrichman/bionic-full-stack-build:0.1.0
skopeo -v copy oci:run   docker-daemon:ghcr.io/enrichman/bionic-full-stack-run:0.1.0
```

```
docker push ghcr.io/enrichman/bionic-full-stack-build:0.1.0
docker push ghcr.io/enrichman/bionic-full-stack-run:0.1.0
```

# builder

```
pack builder create ghcr.io/enrichman/gitea-builder:0.0.4 --config ./builders/gitea-builder/builder.toml
```

```
docker push ghcr.io/enrichman/gitea-builder:0.0.4
```


## deploy

```
epinio app push -p . --name gitea --builder-image ghcr.io/enrichman/gitea-builder:0.0.4
```
