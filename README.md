
# 🚀 **Dashy - Tableau de Bord Auto-Hébergé**

**Dashy** est un tableau de bord moderne et personnalisable qui permet de centraliser l'accès à tes services, applications et liens favoris. Ce fichier `docker-compose.yml` permet d'installer et d'exécuter Dashy avec Docker de manière simple et rapide.

---

### 📌 **Installation et Déploiement**

#### 🔹 **1. Prérequis**  
Avant de commencer, assure-toi d'avoir :  
✅ **Docker** et **Docker Compose** installés sur ton serveur  
✅ Un accès SSH à ton serveur  
✅ Un dossier dédié pour Dashy (comme `/volume1/docker/dashy`)  

#### 🔹 **2. Télécharger et Configurer**  

1. **Créer le répertoire Dashy**  
   Pour créer un dossier dédié à Dashy et y accéder :  
   ```bash
   mkdir -p /volume1/docker/dashy
   cd /volume1/docker/dashy
   ```

2. **Créer le fichier `docker-compose.yml`**  
   Voici le fichier `docker-compose.yml` que tu dois utiliser :  
   ```yaml
   version: '3.9'
   services:
     dashy:
       image: lissy93/dashy:latest
       container_name: Dashy
       restart: on-failure:5
       healthcheck:
         test: ['CMD', 'node', '/app/services/healthcheck']
         interval: 1m30s
         timeout: 10s
         retries: 3
         start_period: 40s
       environment:
         UID: 1026
         GID: 100
         NODE_ENV: production
       volumes:
         - /volume1/docker/dashy/dashyconf.yml:/app/user-data/conf.yml
         - /volume1/docker/dashy/icons:/app/user-data/item-icons:rw
       ports:
         - 7444:8080
   ```

3. **Démarrer Dashy**  
   Lance le conteneur Dashy avec la commande suivante :  
   ```bash
   docker-compose up -d
   ```

4. **Accéder à Dashy**  
   Une fois le conteneur démarré, tu peux accéder à Dashy via ton navigateur en utilisant l'IP de ton serveur :  
   ➡️ **http://IP-DU-SERVEUR:7444**  

---

### ⚙️ **Configuration et Personnalisation**

#### 📝 **Modifier la configuration**  
La configuration principale se trouve dans le fichier suivant :  
```bash
/volume1/docker/dashy/dashyconf.yml
```
Modifie ce fichier selon tes préférences pour personnaliser ton tableau de bord.  
Après modification, redémarre Dashy pour appliquer les changements :  
```bash
docker-compose restart
```

#### 🖼 **Gestion des icônes**  
Tu peux placer tes icônes personnalisées dans le dossier suivant :  
```bash
/volume1/docker/dashy/icons
```
Les icônes doivent être ajoutées dans ce répertoire afin qu'elles soient utilisées dans Dashy.

---

### 🔒 **Sécurité et Accès à Distance**

Si tu souhaites accéder à Dashy depuis Internet :  
1. **Configure un reverse proxy** avec **NGINX** ou **Traefik** pour rediriger le trafic vers Dashy.  
2. **Active l'authentification** pour sécuriser l'accès à ton tableau de bord.  
3. **Ajoute un certificat SSL/TLS** via **Let's Encrypt** pour sécuriser les communications.

---

### 🚀 **Gestion du Conteneur**

Voici quelques commandes utiles pour gérer ton conteneur Dashy :

| Commande | Action |
|----------|--------|
| `docker-compose up -d` | Démarrer le conteneur Dashy en mode détaché |
| `docker-compose down` | Arrêter et supprimer le conteneur |
| `docker-compose restart` | Redémarrer le conteneur |
| `docker logs dashy --tail=50 -f` | Voir les logs du conteneur en temps réel |
| `docker ps` | Vérifier si le conteneur est actif |

---

### 🛠 **Dépannage**

Si Dashy ne démarre pas correctement, voici quelques étapes à suivre pour diagnostiquer le problème :  

1️⃣ **Vérifier les logs du conteneur** pour voir s'il y a des erreurs :  
   ```bash
   docker logs dashy
   ```

2️⃣ **Vérifier si le port est déjà utilisé** (par exemple, le port 7444 utilisé par Dashy) :  
   ```bash
   netstat -tulnp | grep 7444
   ```

3️⃣ **Essayer de redémarrer le conteneur** :  
   ```bash
   docker-compose down && docker-compose up -d
   ```

---

### 🐝 **Sources et Documentation**  
📚 **Docs Officielles** : [https://dashy.to/docs](https://dashy.to/docs)  
🐙 **Repo GitHub** : [https://github.com/Lissy93/dashy](https://github.com/Lissy93/dashy)  

---

### 🐝 **Exécution du Playbook Ansible**

Si tu souhaites automatiser l'installation et la configuration de Dashy sur ton serveur, tu peux utiliser le playbook Ansible suivant :  
```bash
ansible-playbook main.yml -i production
```
Cela automatisera le processus d'installation de Dashy et de son environnement Docker. Assure-toi que ton serveur soit configuré pour accepter les connexions SSH et que les variables nécessaires soient bien définies.

---

🎉 **Félicitations !** Tu as maintenant **Dashy** opérationnel 🚀. Profite de ton tableau de bord personnalisé pour centraliser tous tes services et applications préférés !