# Helm

## TP1

### Installation de helm
```
curl https://baltocdn.com/helm/signing.asc | gpg --dearmor | sudo tee /usr/share/keyrings/helm.gpg > /dev/null
sudo apt-get install apt-transport-https --yes
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/helm.gpg] https://baltocdn.com/helm/stable/debian/ all main" | sudo tee /etc/apt/sources.list.d/helm-stable-debian.list
sudo apt-get update
sudo apt-get install helm
``` 

### Déploiement de Nextcloud via Helm

```
helm repo add nextcloud https://nextcloud.github.io/helm/
helm repo update
```

```
helm repo update
"nextcloud" has been added to your repositories
Hang tight while we grab the latest from your chart repositories...
...Successfully got an update from the "nextcloud" chart repository
Update Complete. ⎈Happy Helming!⎈
```` 

`helm install my-release nextcloud/nextcloud`

```
export APP_HOST=127.0.0.1
export APP_PASSWORD=$(kubectl get secret --namespace default my-release-nextcloud -o jsonpath="{.data.nextcloud-password}" | base64 --decode)

```

```
helm upgrade my-release nextcloud/nextcloud \
  --set nextcloud.password=$APP_PASSWORD,nextcloud.host=$APP_HOST,service.type=ClusterIP,mariadb.enabled=false,externalDatabase.user=nextcloud,externalDatabase.database=nextcloud,externalDatabase.host=YOUR_EXTERNAL_DATABASE_HOST
```

## TP2 - Créer son premier chart

Création des charts

```
helm create nginx-pvig-dev
helm create nginx-pvig-prod
```

On modifie le fichier value pour récupérer l'image avec le tag correspondant.

Création des namespaces :

```
kubectl create namespace production
kubectl create namespace dev
```

Installation des charts

`helm install nginx-pvig nginx-pvig-prod -n production`

```
NAME: my-release
LAST DEPLOYED: Thu Jan 26 14:23:08 2023
NAMESPACE: production
STATUS: deployed
REVISION: 1
NOTES:
1. Get the application URL by running these commands:
  export NODE_PORT=$(kubectl get --namespace production -o jsonpath="{.spec.ports[0].nodePort}" services nginx-chart-prod)
  export NODE_IP=$(kubectl get nodes --namespace production -o jsonpath="{.items[0].status.addresses[0].address}")
  echo http://$NODE_IP:$NODE_PORT
```

## Ajout de contenu

Pour remplacer le contenu du fichier index.html on peut utiliser un fichier configmap.yml :

```
apiVersion: v1
kind: ConfigMap
metadata:
  name: {{ .Release.Name }}-index-html-configmap
  namespace: default
data:
  index.html: |
    <html>
    <h1>Welcome</h1>
    </br>
    <h1>Hi! I got deployed in {{ .Values.env.name }} Environment using Helm Chart </h1>
    </html```
