## Install

### Helm Install

```sh
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm repo update
helm upgrade --install ingress-nginx/ingress-nginx -f values.yaml --namespace ingress-nginx --create-namespace --version 4.7.1
```

## Reference

### Sample Example

- https://github.com/kubernetes/ingress-nginx/tree/main/docs/examples

### Compatible Version

|  Supported  | Ingress-NGINX version | k8s supported version        | Alpine Version | Nginx Version | Helm Chart Version |
|:--:|-----------------------|------------------------------|----------------|---------------|------------------------------|
| 🔄 | **v1.9.6**            | 1.29, 1.28, 1.27, 1.26, 1.25        | 3.19.0         | 1.21.6        | 4.9.1*                 |
| 🔄 | **v1.9.5**            | 1.28, 1.27, 1.26, 1.25        | 3.18.4         | 1.21.6        | 4.9.0*                       |
| 🔄 | **v1.9.4**            | 1.28, 1.27, 1.26, 1.25        | 3.18.4         | 1.21.6        | 4.8.3                        |
| 🔄 | **v1.9.3**            | 1.28, 1.27, 1.26, 1.25        | 3.18.4         | 1.21.6        | 4.8.*                        |
| 🔄 | **v1.9.1**            | 1.28, 1.27, 1.26, 1.25        | 3.18.4         | 1.21.6        | 4.8.*                        |
| 🔄 | **v1.9.0**            | 1.28, 1.27, 1.26, 1.25        | 3.18.2         | 1.21.6        | 4.8.*                        |
| 🔄 | **v1.8.4**            | 1.27, 1.26, 1.25, 1.24        | 3.18.2         | 1.21.6        | 4.7.*                        |
| 🔄 | **v1.8.2**            | 1.27, 1.26, 1.25, 1.24        | 3.18.2         | 1.21.6        | 4.7.*                        |
| 🔄 | **v1.8.1**            | 1.27, 1.26, 1.25, 1.24        | 3.18.2         | 1.21.6        | 4.7.*              |
| 🔄 | **v1.8.0**            | 1.27, 1.26, 1.25, 1.24        | 3.18.0         | 1.21.6        | 4.7.*              |
|  | **v1.7.1**            | 1.27, 1.26, 1.25, 1.24        | 3.17.2         | 1.21.6        | 4.6.*              |
|  | **v1.7.0**            | 1.26, 1.25, 1.24             | 3.17.2         | 1.21.6        | 4.6.*              |
|    | v1.6.4                | 1.26, 1.25, 1.24, 1.23       | 3.17.0         | 1.21.6        | 4.5.*              |
|    | v1.5.1                | 1.25, 1.24, 1.23             | 3.16.2         | 1.21.6        | 4.4.*              |
|    | v1.4.0                | 1.25, 1.24, 1.23, 1.22       | 3.16.2         | 1.19.10†      | 4.3.0              |
|    | v1.3.1                | 1.24, 1.23, 1.22, 1.21, 1.20 | 3.16.2         | 1.19.10†      | 4.2.5              |
|    | v1.3.0                | 1.24, 1.23, 1.22, 1.21, 1.20 | 3.16.0         | 1.19.10†      | 4.2.3              |

### Configuration

- https://github.com/kubernetes/ingress-nginx/blob/main/charts/ingress-nginx/README.md

### Annotation

- https://github.com/kubernetes/ingress-nginx/blob/main/docs/user-guide/nginx-configuration/annotations.md