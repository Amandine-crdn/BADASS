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

## P3:

Topologie du réseau : 1 Spine et 3 Leafs
### Interface eth0
- Desactiver le routage ivp6
- Attribution d'une IP sur le meme subnet 10.1.1.1/30
- Configuration interface pour utiliser protocole routage OSPF (backbone zone)

### Backbone zone (aire OSPF 0)


### Interface lo (loopback) 
Interface virtuelle,  toujours disponible (pas de panne comme eth0) tant que l'équipement est opérationnel. Elle permet donc d'avoir une adresse IP stable, souvent utilisée pour identifier le routeur dans les protocoles de routage (comme BGP et OSPF)., permet d'acceder a distance au routeur: identité stable du routeur pour les communications BGP et OSPF.

### BGP (Border Gateway Protocol) 
- Activer le BGP avec l'AS (Autonomous System) numéro 1.
- Configure un voisin BGP avec l'adresse 1.1.1.1, qui appartient également à l'AS 1 (ce qui signifie qu'il s'agit d'un BGP interne, ou iBGP).
- Spécifie que les mises à jour BGP doivent utiliser l'interface loopback comme source.

### Address family l2vpn evpn 
- Active l'adresse familiale l2vpn evpn pour le BGP EVPN, qui est utilisé dans des environnements où l'on transporte des informations de niveau 2 (Ethernet) à travers BGP.
- Active le voisin BGP 1.1.1.1 pour cette adresse familiale.
- Annonce tous les VNI (VXLAN Network Identifiers) au voisin.

### VXLAN avec BGP EVPN :

    Nous avons configuré BGP pour assurer la communication de niveau 3 (IP) entre les routeurs.
    Nous avons utilisé VXLAN pour transporter des réseaux de niveau 2 (comme un LAN ou un VLAN) au-dessus de l'infrastructure niveau 3 (IP).
    BGP EVPN a été utilisé pour distribuer les informations VXLAN (comme les adresses MAC et les informations de routage IP) entre les routeurs.

### OSPF :

    Chaque routeur est également configuré avec OSPF pour assurer le routage interne dans l'aire 0 (backbone).
    OSPF assure que toutes les routes sont bien propagées entre les routeurs au sein du réseau local.