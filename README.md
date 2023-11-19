# flyte-on-pi

## Setting up raspberry pi with ubuntu server image
- stuff
## Install microk8s
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

## Ingress
### Create self signed certificate
- `KEY_FILE=flyte.key`
- `CERT_FILE=flyte.crt`
- `CERT_NAME=flyte`
- `HOST=flyte.local`
- `openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout ${KEY_FILE} -out ${CERT_FILE} -subj "/CN=${HOST}/O=${HOST}" -addext "subjectAltName = DNS:${HOST}"`
### Create k8s secret
- `kubectl create secret tls home --key ${KEY_FILE} --cert ${CERT_FILE} -n flyte`
### Adjust helm chart
``` yaml
ingress:
  create: true
  host: flyte.local
  ingressClassName: public
  separateGrpcIngress: true
  tls:
    - hosts:
        - flyte.local
      secretName: home
  commonAnnotations:
    kubernetes.io/ingress.class: public
  httpAnnotations:
    nginx.ingress.kubernetes.io/app-root: /console
  grpcAnnotations:
    nginx.ingress.kubernetes.io/backend-protocol: GRPC
```
## Running Flyte
### run from pi
- `kubectl -n flyte port-forward service/flyte-binary-http 8088:8088`
- `kubectl -n flyte port-forward service/flyte-binary-grpc 8089:8089`
### running from within LAN
- add `192.168.178.68  flyte.local` to /etc/hosts (or configure some local DNS)
- configure `~./flyte/config.yaml`
``` yaml
admin:
  endpoint: dns:///flyte.local
  authType: Pkce
  insecure: false
  insecureSkipVerify: false
  caCertFilePath: /Users/janfiedler/Documents/flyte/flyte-on-pi/flyte.local.cer
```


## Next TO DOS
- logs grafana?
- core dns entry?
```
little-guy:53 {
		    errors
		    hosts {       
		        192.168.178.7 little-guy       
		    }
		}
```