# Install Ingress Nginx with letsencrypt certificates

Guide from [DigitalOcean](https://www.digitalocean.com/community/tutorials/how-to-set-up-an-nginx-ingress-with-cert-manager-on-digitalocean-kubernetes)

## Install backend services

```sh
kubectl create -f kube/deployments/echo1.yaml
kubectl create -f kube/services/echo1.yaml
```

## Install Ingress Nginx

```sh
helm install stable/nginx-ingress --name helm-ingress --namespace ingress-nginx
```

## Install cert-manager

```sh
kubectl apply -f https://raw.githubusercontent.com/jetstack/cert-manager/release-0.8/deploy/manifests/00-crds.yaml
helm repo add jetstack https://charts.jetstack.io
helm install --name helm-cert --namespace cert-manager jetstack/cert-manager
kubectl label namespace cert-manager certmanager.k8s.io/disable-validation="true"
kubectl create -f kube/issuers/production_issuer.yaml
```

## Apply Ingress objects

```sh
kubectl apply -f kube/ingresses/echo-ingress.yaml
kubectl describe ingress
kubectl describe certificate
curl https://echo1.lozuk.dev -v
```
