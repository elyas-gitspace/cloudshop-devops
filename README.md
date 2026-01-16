### **CloudShop - Plateforme E-commerce Microservices DevOps**

__📋 Table des Contenus:__

```
- 🎯 Présentation du Projet

- 🔄 Workflow Global

- 🛠️ Workflow Technique

- 🚀 Commandes de Déploiement

- 🔧 Technologies Utilisées

- 📊 Architecture
```

__Présentation du Projet:__

```CloudShop est un projet DevOps complet qui simule le cycle de vie d'une application e-commerce moderne, depuis le développement jusqu'au déploiement en production```

__Objectif Principal:__

```
Démontrer les compétences DevOps en construisant une infrastructure complète :

- 3 microservices Flask (Frontend, Product API, Order API)

- Conteneurisation avec Docker

- Orchestration avec Kubernetes (Minikube)

- CI/CD avec GitHub Actions
```

__⚠️ Note Importante sur l'Automatisation:__

```
GitHub Actions est utilisé uniquement pour les tests et la validation, tandis que le déploiement se fait manuellement sur Minikube localement

Cette approche est choisie pour :

- Économie de coûts : Pas besoin d'un cluster Kubernetes cloud coûteux

- Apprentissage : Compréhension profonde des commandes manuelles

- Sécurité : Pas d'exposition de cluster sur internet

Le principe reste identique à une vraie production : si nous avions un cluster cloud (AWS EKS, Google GKE), GitHub Actions déploierait automatiquement
```

### **Workflow Global**
__Vue d'ensemble du processus:__

```
graph TD
    A[Développement local] --> B[Push sur GitHub]
    B --> C[GitHub Actions CI]
    C --> D{Tests automatiques}
    D --> E[✅ Validation réussie]
    D --> F[❌ Échec - Notification]
    E --> G[Rapport généré]
    G --> H[Déploiement MANUEL sur Minikube]
    
    subgraph "Tests GitHub Actions"
        C --> D1[Tests Python]
        C --> D2[Validation Dockerfiles]
        C --> D3[Validation YAML K8s]
    end
    
    subgraph "Déploiement Manuel"
        H --> I[Build images dans Minikube]
        I --> J[kubectl apply]
        J --> K[Application en ligne]
    end

```

__Étapes clés :__

```
- __Développement :__ Code des microservices Flask

- __CI Automatisée :__ GitHub Actions teste tout automatiquement

- __Validation :__ Dockerfiles et manifests Kubernetes vérifiés

- __Déploiement Manuel :__ Commandes exécutées localement sur Minikube
```

### **🛠️ Workflow Technique**

__1. Phase de Développement (Local)__

```
- Écriture du code Python
- Test avec Docker Compose
   docker-compose up -d
- Vérification : http://localhost:5000
```
__2. Phase CI Automatisée (GitHub Actions)__

```
yaml
 .github/workflows/ci-cd.yml
name: CI/CD Pipeline CloudShop

on: [push]  # Déclenché à chaque git push

jobs:
validate-and-test:
steps:
- Tests unitaires Python
- Validation syntaxe Dockerfiles
- Validation syntaxe YAML Kubernetes  # --dry-run uniquement
    
build-and-deploy:
if: push sur main
steps:
- Build images Docker
- Simulation déploiement  # Écho seulement, pas de vrai déploiement
- Génération rapport

__Points clés de GitHub Actions :__

✅ Validation Dockerfiles : Vérifie que les Dockerfiles sont syntaxiquement corrects

✅ Validation Kubernetes : Vérifie que les fichiers YAML sont valides (--dry-run)

Pas de vrai déploiement : Pas de cluster K8s accessible sur GitHub
```

__3. Phase de Déploiement Manuel (Local - Minikube)__

```
bash
- Démarrer l'environnement
_minikube start --driver=docker_

- Construire dans Minikube
_minikube image build -t frontend:latest ./frontend_

- Déployer sur Kubernetes
_kubectl apply -f kubernetes/_

- Accéder à l'application
_minikube service frontend-service --url_
🚀 Commandes de Déploiement
📋 Cheat Sheet des Commandes Essentielles
```

### **Initialisation et Setup:**
```
_bash_

__Démarrer Minikube (cluster Kubernetes local):__

_minikube start --driver=docker_

__Vérifier l'état:__

_kubectl cluster-info_
_minikube status_
```

### **Construction des Images:**

```
_bash_

__Méthode via le Docker de Minikube:__

_minikube docker-env_
(Exécuter la commande affichée)
puis

_docker build -t frontend:latest ./frontend_

```

### **Déploiement sur Kubernetes:**


_bash_

__Appliquer toutes les configurations:__

_kubectl apply -f kubernetes/_

### **Accès aux Services:**

_bash_

__Frontend (interface web):__

_kubectl port-forward service/frontend-service 8080:80_

__Navigateur http://localhost:8080__

__Product API (catalogue):__

_kubectl port-forward service/product-api-service 5001:5000_

__Tester : curl http://localhost:5001/products__

__Order API (commandes):__

_kubectl port-forward service/order-api-service 5002:5000_

__Tester : curl http://localhost:5002/orders__