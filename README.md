# Chart Helm de démonstration DSO

## Présentation

Ce chart HELM permet de déployer l'application de [démonstration static](https://github.com/dnum-mi/dso-tuto-static)

Ce chart permet de créer :
 - Création d'un déploiement de l'image construite par DSO sur le projet [démonstration static](https://github.com/dnum-mi/dso-tuto-static)
 - Création d'un service sur le déploiement
 - Création d'un ingress en https vers le service

Ce Chart peut servir dans le cas d'un test de déploiement comme repo d'infrastructure depuis la console DSO.

## Variables

Fichier de variable minimal:

```
image:
  repository: quay.apps.ocp4-8.infocepo.com/ministere-interieur-demo/app-nginx-static-demo # Référence de l'image DSO à déployer

ingress:
  enabled: true # Création d'un ingress
  secret:
    enabled: false # Création du secret TLS false si déjà présent
  host: dso-demo-static-helm.apps.ocp4-8.infocepo.com # Nom d'host
  tls:
    - secretName: dso-demo-static-tls # Nom du secret contenant le certificat TLS
``` 

Fichier de variable complet :
```
replicaCount: 1 # Nombre de réplicas du pod

image:
  repository: quay.apps.ocp4-8.infocepo.com/ministere-interieur-demo/app-nginx-static-demo # Nom de l'image DSO à déployer, dépendant de l'organisation et du projet DSO créé
  pullPolicy: Always # Policy de récupération de l'image sur le node (Always : à chaque déploiement de pod, IfNotPresent : uniquement sur un node qui ne contient pas l'image en cache)
  tag: "master" # Nom du tag de l'image

imagePullSecrets: 
  - name: "quay-image-pull-secret" # Nom du secret permettant de se connecter à la registry Quay. Ne pas modifier cette valeur dans le cadre de DSO

service:
  type: ClusterIP # Type de service kubernetes à créer, ClusterIP dans la majorité des cas
  port: 8080 # Port d'exposition du service kubernetes

ingress:
  enabled: true # Création d'un ingress
  secret:
    enabled: false # Création du secret TLS false si déjà présent
  host: dso-demo-static-helm.apps.ocp4-8.infocepo.comm # Nom d'host
  tls:
    - secretName: dso-demo-java-tls # Nom du secret contenant le certificat TLS

resources: # Contraintes limit et requests sur les ressources du POD
  limits:
    memory: "512Mi"
    cpu: "500m"  
  requests:
    memory: "512Mi"
    cpu: "500m"  

```
