# Helm

## Installation de helm
```
curl https://baltocdn.com/helm/signing.asc | gpg --dearmor | sudo tee /usr/share/keyrings/helm.gpg > /dev/null
sudo apt-get install apt-transport-https --yes
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/helm.gpg] https://baltocdn.com/helm/stable/debian/ all main" | sudo tee /etc/apt/sources.list.d/helm-stable-debian.list
sudo apt-get update
sudo apt-get install helm
``` 

## Déploiement de Nextcloud via Helm

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