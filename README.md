
### Docker project

Ce projet comprend plusieurs stacks Docker Compose, notamment Nextcloud, MyApp, Portainer et un proxy inverse Nginx pour les certificats SSL et les redirections.

### Stack Nextcloud
La stack Nextcloud est un service répliqué utilisant Docker Swarm pour l'équilibrage de charge. Il est connecté à une base de données et est composé de deux répliques. La stack s'appelle "nextcloud".

### Stack MyApp
La stack MyApp est une application web simple qui affiche "Hello World". Elle est composée d'un conteneur unique et la stack s'appelle "myapp".

### docker Portainer
La stack Portainer est utilisée pour gérer et surveiller les services Docker. Elle est composée d'un conteneur unique et la stack s'appelle "portainer".

### docker Nginx Reverse Proxy
La stack Nginx reverse proxy est utilisée pour gérer les certificats SSL et les redirections vers les services Nextcloud et MyApp. Elle est composée d'un conteneur unique et la stack s'appelle "nginx-reverse-proxy".

Pour plus d'informations sur chaque stack, veuillez consulter les fichiers Docker Compose correspondants dans les répertoires respectifs.

Remarque : Assurez-vous d'avoir Docker et Docker Compose installés sur votre système avant de déployer les stacks.

Pour démarrer les docker, naviguez vers les répertoires respectifs et exécutez la commande docker-compose up -d. Pour arrêter les docker, exécutez la commande docker-compose down.


Pour utiliser les stack avec Docker Swarm, voici les prérequis :

1. Assurez-vous d'avoir Docker et Docker Compose installés sur votre système.
2. Activez Docker Swarm en exécutant la commande suivante :
```
docker swarm init
```
3. Pour déployer une stack, naviguez vers le répertoire correspondant et exécutez la commande suivante :
```css
docker stack deploy -c docker-compose.yml <nom_de_la_stack>
```
Remplacez `<nom_de_la_stack>` par le nom de la stack que vous souhaitez déployer. Par exemple, pour déployer la stack Nextcloud, exécutez la commande :
```perl
docker stack deploy -c docker-compose.yml nextcloud
```
4. Pour vérifier l'état des services de la stack, exécutez la commande :
```css
docker service ls
```
5. Pour supprimer une stack, exécutez la commande :
```perl
docker stack rm <nom_de_la_stack>
```
Note : Avant de déployer les stacks, assurez-vous que les ports nécessaires sont ouverts sur votre réseau et que les variables d'environnement sont correctement configurées dans les fichiers Docker Compose.

![shema_infra](https://github.com/Naililruojnob/Docker-Project/blob/592daf236269fcd2ffb712d558828372e73458ee/Shema_infra.png)

image myapp https://hub.docker.com/repository/docker/naililruojnob/myapp/general


#### ELK

Pour utiliser le service Fleet, il faut le configurer directement sur Kibana en modifiant ses outputs pour qu'ils pointent vers le serveur Elasticsearch.

Il est également nécessaire d'ajouter le "Elasticsearch CA trusted fingerprint". Pour ce faire, nous devons d'abord copier le certificat Elasticsearch avec la commande suivante :

```bash
docker cp <nom_du_conteneur_elasticsearch>:/usr/share/elasticsearch/config/certs/ca/ca.crt /tmp/.
```

Ensuite, pour récupérer son fingerprint, nous devons exécuter la commande suivante :

```bash
openssl x509 -fingerprint -sha256 -noout -in /tmp/ca.crt | awk -F"=" '{print $2}' | sed s/://g
```

Enfin, nous devons formater le certificat sous forme avancée YAML, comme illustré dans l'exemple ci-dessous :

```yaml
ssl:
  certificate_authorities:
  - |
    -----BEGIN CERTIFICATE-----
    MIIDSjCCAjKgAwIBAgIVAKpPwDICQZrrgcKolZqcTTNE+RIaMA0GCSqGSIb3DQEB
    CwUAMDQxMjAwBgNVBAMTKUVsYXN0aWMgQ2VydGlmaWNhdGUgVG9vbCBBdXRvZ2Vu
    ZXJhdGVkIENBMB4XDTI0MDMwNzEwMTY1NFoXDTI3MDMwNzEwMTY1NFowNDEyMDAG
    A1UEAxMpRWxhc3RpYdddddddddsddddddddsdsdscEF1dG9nZW5lcmF0ZWQgQ0Ew
    ggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAwggEKAoIBAQC5lScbKu9W/Atzl8WUscFy
    e/L1ixPT2fsugSklaoCC/fasu9GRkiPUEW1jPD2czFzfS2H/D9/gXcIOeo6sDIf2
    YcHP5e44lf4b7/7SVmO7jJS+ccccccccccccLfmYlckSOJhfTEDVfy/qi3kBSiiq
    8yjE3d+wHUxy/QZbsSlfAHJ/fU0d6mfiIk33iefMHjBrbaCGg9Gdwxp0+eegTbg7
    Nqg+u2CqtlVQ5bTSUnZiX0tyP0YtkxqWW2aiMNEEfqQXAFIdYZ81K0IvGk2ht4Xd
    AgMBAAGjUzBRMB0GA1UdDgQWBBTDc6VXwGEcEdvDFHzOTpFcKcuY1DAfBgNVHSME
    GDAWgBTDc6VXwGEcEdvDFHzOTpFcKcuY1DAPBgNVHRMBAf8EBTADAQH/MA0GCSqG
    SIb3DQEBCwUAA4IBAQB96YywB1M4BcC/aQUdvILcAz8bsfHXssovQcIBsgmGUzO4
    s4pTk/0fVJV+HMBWxx9KMW2omjEPNthuF8B5QUuzboyF3SENi23QHFR6USOO+Rve
    2HhW9KndW+qSZqc3WXuLyEVB6e+CzztewMHPr4M+4Dj2yyjXJwBYEXu47yabSRo4
    4cHRRbRr2C2gxK36wwpsDqTulsauOhlWkOMNdPsIWlKKYUjhwrlYkjlCu+8WT48i
    6HhQ4cXFKQOdsXCAau6Mu05OX0du8ipUs6cJy37GmBSJ4FhrDLSBf6P1ICqVAQ3q
    mCwJg8nmPtEcWgzVAH1N4dwlK5VlegVdgr9vQUWY
    -----END CERTIFICATE-----

