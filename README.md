# registry

Install registry:
```bash
kubectl create deployment registry --image=registry
kubectl expose deployment registry --port=5000 --name=registry
```

Setup ingress:
```bash
kubectl apply -f - << EOF
apiVersion: networking.k8s.io/v1beta1
kind: Ingress
metadata:
  name: registry
spec:
  tls:
    - hosts:
      - registry.k8s.shubhamtatvamasi.com
      secretName: letsencrypt
  rules:
    - host: registry.k8s.shubhamtatvamasi.com
      http:
        paths:
        - path: /
          backend:
            serviceName: registry
            servicePort: 5000
EOF
```

Delete everything:
```bash
kubectl delete deploy/registry svc/registry ing/registry
```

Test registry:
```bash
docker pull busybox
docker tag busybox registry.k8s.shubhamtatvamasi.com/busybox
docker push registry.k8s.shubhamtatvamasi.com/busybox

docker rmi registry.k8s.shubhamtatvamasi.com/busybox
docker pull registry.k8s.shubhamtatvamasi.com/busybox
```
