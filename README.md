# BADASS
Creer sa VM
Install docker
Install GNS3: https://superuser.com/questions/1788720/how-to-install-gns3-on-debian12

## P1: GNS3 configuration with Docker.
Creer un environnement Python et installer gns3
Creer 2 images, chaque image representera une vm: l'un un gateway l'autre un host
Pour le routeur on a decider de pull de dockerhub Frrrouting, et pour le host Alpine (busy box integrer)

Creer les images docker
```docker build -t image_name -f dockerfile_name .```

Ouvrir GNS3 dans l'env python
Aller dans les preferences et ajouter vos machines via les ajouts docker

Creer votre topographie en inserant les machines souhaitees.


## P2: Discovering a VXLAN.

Pour les hosts:
- Attribution d'une IP sur le meme subnet 30.0.0.0/24
- Activation de leur interface eth1

Pour les gateways:
- Attribution d'une IP sur le meme subnet 10.0.0.0/24
- Créer une interface bridge br0 et l'activer
- Créer une interface VxLAN vxlan10, l'activer, lui attribuer une IP 20.1.1.1/24
- Relier grace au bridge vxlan10 et eth1

Pour la partie static -> dynamique : au lieu de relier les 2 gateways, on definit un groupe







On lance GNS3, on creer nos VM (2 gateways, 2 hosts)
dynamic multicast