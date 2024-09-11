# ☁️ Cloud index

- https://docs.microsoft.com/fr-fr/azure/developer/javascript/?view=azure-node-latest&ocid=AID793987_TWITTER_oo_spl100000748467392
- https://cri-o.io/
- https://containerd.io/
- https://speakerdeck.com/govargo/inside-of-kubernetes-controller
- https://kubernetes.io/fr/docs/tutorials/kubernetes-basics/
- https://k8slens.dev/index.html

## Docker

### Good defaults

https://github.com/BretFisher/node-docker-good-defaults

### Kata containers

https://github.com/kata-containers/

## Kubernetes on-prem

# Kubernetes on premise

The hard way

- Effet d’oeuf et de poule
- Souvent No man’s land

Architecture Kube

Composant à installer

Installation à la main :

- Kubeadm

Installation automatisé Configuration Management (ansible, chef, puppet) :

- Intégré
- Fastidieux
- Maintenance laborieuse

Pseudo configuration management :

- Kubespray : Ansible pour Kubernetes pré-packagé
  - Simple d’usage
  - Usine à gaz

Outils dédié :

- Kops
  - Compatibilité -
- RKE :
  - Compatibilité ++
  - Intégré avec rancher
  - Assez dégueulasse

Cloud privé :

- OpenStack
  - Kubernikus
    - KaaS
  - Metal3
    - BareMetal avec Ironic
  - Zun
    - Nodeless

VMWare :

- vSphere 7 / Tanzu
  - Tans-kubernetes-grid

PKS (Pivotal Kubernetes Service)

- Pivotal provisionner avec BOSH

Kubernetes Way©

- ClusterAPI + MachineAPI
  - Déclaration des clusters
  - Nécessite un provider qui fonctionne
- Gardener
  - Seed Cluster
    - Control plane
  - Cloud / Bare Metal
    - Worker
- KINKy
  - Kube in Kube
    - Kub opérateur de Kube
    - Worker rejoint avec kubeadm join

Valorisation

- MetalLB équivalent d’un keepalive
  - Routes via L3/BGP
  - Déclare une IP publique d’un service
- Rook (Stockage : Volumes, Volume locaux)
  - Opérateur Kubernetes de Ceph
    - Block
    - Objet
    - Système de fichier

## Providers

- [Clever Cloud](https://www.clever-cloud.com/)
- https://github.com/mikeroyal/AWS-Guide
- [Azure](https://learn.microsoft.com/fr-fr/azure/developer/javascript/?view=azure-node-latest&ocid=AID793987_TWITTER_oo_spl100000748467392)
- GKE
- EKS
- AKS
- OVH
  - basé sur OpenStack
  - Lancé très tardivement
- Scaleway

  Un accès différents mais des primitives identiques (si on maîtrise K8s on maîtrise dans les grandes lignes les solution adossés)
  Primitives :

  - Autoscaler : ne pas avoir à gérer l'extensibilité du cluster / fait du downscale
  - Node-pool : preemptive / spot, GPU?
  - Nodeless : ne pas avoir à gérer des nœuds (+++ : sécurisation & mise à jour) / scalability instantanée / exemples : ACI, AWS fargate
  - Ingress HTTP : intégré : cher, fonctionnalité ++ (certificats, WAF), simple /// customer : + portable
  - External-dns: auto-peuple vos DNS à partir de vos vhosts // supporté par votre fournisseur de DNS et par vos cloud provider?

## Tools

https://istiobyexample.dev/
