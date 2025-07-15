# Cheat Sheet Docker Essentiel

Cette cheat sheet contient les commandes et concepts Docker fondamentaux pour vous aider à démarrer rapidement.

---

## I. Introduction à Docker

* **Qu'est-ce que Docker ?**
    Une plateforme qui permet aux développeurs de développer, déployer et exécuter des applications dans des conteneurs. Un conteneur est une unité standardisée de logiciel qui packtise votre code et toutes ses dépendances, permettant à l'application de s'exécuter rapidement et de manière fiable d'un environnement à un autre.

* **Concepts Clés :**
    * **Image :** Un modèle léger, autonome et exécutable qui inclut tout ce dont vous avez besoin pour exécuter une application (code, runtime, bibliothèques, variables d'environnement, fichiers de configuration).
    * **Conteneur :** Une instance exécutable d'une image. Vous pouvez penser à une image comme une classe et à un conteneur comme une instance de cette classe.
    * **Dockerfile :** Un fichier texte qui contient toutes les commandes nécessaires pour construire une image Docker.
    * **Docker Hub :** Un service cloud où vous pouvez trouver et partager des images Docker.

---

## II. Commandes Essentielles Docker

### Gestion des Images

* **`docker pull [image_name]:[tag]`**
    Télécharge une image depuis Docker Hub (ex: `docker pull ubuntu:latest`).
* **`docker images`**
    Liste toutes les images locales présentes sur votre machine.
* **`docker rmi [image_id_or_name]`**
    Supprime une image spécifique par son ID ou son nom (ex: `docker rmi ubuntu:latest`).
* **`docker build -t [image_name]:[tag] .`**
    Construit une image à partir d'un Dockerfile dans le répertoire courant (le `.` signifie le répertoire actuel). (ex: `docker build -t monapp:1.0 .`).

### Gestion des Conteneurs

* **`docker run -d -p [host_port]:[container_port] --name [container_name] [image_name]:[tag]`**
    Démarre un nouveau conteneur en mode détaché (`-d`), mappe un port de votre machine hôte au port du conteneur (`-p`), et lui donne un nom (`--name`).
    (ex: `docker run -d -p 80:80 --name monwebserver nginx`).
* **`docker run -it [image_name]:[tag] /bin/bash`**
    Démarre un conteneur en mode interactif (`-i`) avec un pseudo-TTY (`-t`), et ouvre un shell Bash à l'intérieur. Utile pour explorer le conteneur.
    (ex: `docker run -it ubuntu:latest /bin/bash`).
* **`docker ps`**
    Liste tous les conteneurs en cours d'exécution.
* **`docker ps -a`**
    Liste tous les conteneurs (en cours d'exécution et arrêtés).
* **`docker start [container_id_or_name]`**
    Démarre un conteneur arrêté.
* **`docker stop [container_id_or_name]`**
    Arrête un conteneur en cours d'exécution.
* **`docker restart [container_id_or_name]`**
    Redémarre un conteneur.
* **`docker rm [container_id_or_name]`**
    Supprime un conteneur. Vous devez l'arrêter avant de le supprimer.
* **`docker logs [container_id_or_name]`**
    Affiche les logs (sorties standards) d'un conteneur.
* **`docker exec -it [container_id_or_name] /bin/bash`**
    Exécute une commande (ici `/bin/bash`) dans un conteneur déjà en cours d'exécution. Utile pour interagir avec un conteneur sans le redémarrer.

### Gestion des Volumes

* **`docker volume create [volume_name]`**
    Crée un volume persistant pour stocker des données en dehors des conteneurs.
* **`docker volume ls`**
    Liste tous les volumes Docker.
* **`docker volume inspect [volume_name]`**
    Affiche des informations détaillées sur un volume.
* **`docker volume rm [volume_name]`**
    Supprime un volume.

    *Pour utiliser un volume avec un conteneur :*
    `docker run -v [volume_name]:[container_path] [image_name]`
    (ex: `docker run -d -v mydata:/app/data --name myapp myimage`).

### Gestion des Réseaux

* **`docker network ls`**
    Liste tous les réseaux Docker.
* **`docker network create [network_name]`**
    Crée un nouveau réseau bridge défini par l'utilisateur.

    *Pour attacher un conteneur à un réseau :*
    `docker run --network [network_name] [image_name]`
    (ex: `docker run -d --network my-app-network --name mybackend mybackendimage`).

---

## III. Dockerfile - Bases

Un **Dockerfile** est un script qui contient une série d'instructions pour construire une image Docker. Chaque instruction crée une couche dans l'image.

**Structure de base :**

```dockerfile
# Commentaire
FROM ubuntu:latest             # Image de base sur laquelle vous construisez
WORKDIR /app                    # Définit le répertoire de travail pour les instructions suivantes
COPY . /app                     # Copie les fichiers du répertoire hôte vers le conteneur
RUN apt-get update && apt-get install -y python3 # Exécute des commandes pendant la construction de l'image
EXPOSE 80                       # Informe Docker que le conteneur écoute sur le port 80 à l'exécution
CMD ["python3", "app.py"]       # Commande par défaut à exécuter quand le conteneur démarre