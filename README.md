# flyte-on-pi

## Setting up raspberry pi with ubuntu server image
- stuff
## Install micromk8s
- stuff
- add `alias kubectl="microk8s kubectl"` to `~/.bashrc`
## Logging into pi

- `ssh ubuntu@cluster`
- `ubuntu`

## Install Flyte
- `helm upgrade flyte-binary flyteorg/flyte-binary  --values flyte-on-pi/binary/local-values.yaml --install --create-namespace -n flyte`

## microk8's stuff

- `microk8s status`
- `microk8s enable <addon>`
- `microk8s dashboard-proxy`

## Running Flyte
- `PATH=/home/ubuntu/.local/bin`
### run from pi
- `kubectl -n flyte port-forward service/flyte-binary-http 8088:8088`
- `kubectl -n flyte port-forward service/flyte-binary-grpc 8089:8089`

## Next TO DOS
- core dns entry
```
little-guy:53 {
		    errors
		    hosts {       
		        192.168.178.7 little-guy       
		    }
		}
```

- ingress