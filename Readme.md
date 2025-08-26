# 🌸 MLOps Project – Iris Flower Classification

## 📌 Description
Ce projet met en place une application complète de **Machine Learning** et **MLOps**.  
L’objectif est de prédire la classe d’une fleur (*Iris-setosa, Iris-versicolor, Iris-virginica*) à partir de mesures de sépales et pétales.  

L’application intègre plusieurs volets : **ML**, **Web App**, **Containerisation**, et **Déploiement Cloud automatisé**.  

---

## 🧠 Machine Learning
1. **Prétraitement & entraînement**
   - Jeu de données : Iris Dataset
     ```
     Id,SepalLengthCm,SepalWidthCm,PetalLengthCm,PetalWidthCm,Species
     1,5.1,3.5,1.4,0.2,Iris-setosa
     52,6.4,3.2,4.5,1.5,Iris-versicolor
     102,5.8,2.7,5.1,1.9,Iris-virginica
     ```
   - Prétraitement des données (nettoyage, formatage)  
   - Modèle : **DecisionTreeClassifier** de `scikit-learn`  
   - Sauvegarde du modèle pour utilisation dans l’application Flask

2. **Application Web**
   - Développée avec **Flask**  
   - Interface HTML/CSS avec un formulaire pour saisir les caractéristiques d’une fleur  
   - Soumission du formulaire déclenche le modèle pour effectuer la prédiction en temps réel  

---

## 🐳 Conteneurisation
- Création d’un `Dockerfile` pour packager l’application Flask et le modèle ML  
- Build local de l’image Docker pour garantir portabilité et reproductibilité  

---

## ☁️ Déploiement Cloud & MLOps

### Architecture CI/CD
- **GitHub** : source du code  
- **CircleCI** : pipeline automatisée pour builder et déployer l’application  
- **Google Cloud** : Artifact Registry et cluster Kubernetes (GKE)  

### Pipeline CircleCI (`.circleci/config.yml`)
Le workflow est composé de trois jobs principaux :

1. **checkout_code**
   - Récupération du code source depuis GitHub

2. **build_docker_image**
   - Installation du client Docker
   - Authentification sur Google Cloud via `GCLOUD_SERVICE_KEY`
   - Build de l’image Docker de l’application
   - Push de l’image vers **Google Artifact Registry**

3. **deploy_to_gke**
   - Installation du client Docker
   - Authentification sur Google Cloud
   - Configuration du cluster GKE
   - Déploiement de l’application via le fichier `kubernetes-deployment.yaml`  

**Workflow complet :**

workflows:
  version: 2
  deploy_pipeline:
    jobs:
      - checkout_code
      - build_docker_image:
          requires:
            - checkout_code
      - deploy_to_gke:
          requires:
            - build_docker_image

Grâce à ce pipeline, tout push sur GitHub déclenche automatiquement la build de l’image et son déploiement sur GKE, garantissant un flux CI/CD complet
            
## 🛠 Technologies utilisées
- Python, Flask
- Scikit-learn
- Docker
- Google Cloud Platform (Artifact Registry, GKE)
- Kubernetes
- GitHub
- CircleCI

