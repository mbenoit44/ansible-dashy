## 🚀 **Dashy - Tableau de Bord Auto-Hébergé**  

**Dashy** est un tableau de bord moderne et personnalisable permettant de centraliser l'accès à tes services, applications et liens favoris. Ce fichier `docker-compose.yml` permet d'installer et d'exécuter Dashy avec Docker.  

---

### 📌 **Installation et Déploiement**  

#### 🔹 **1. Prérequis**  
Avant de commencer, assure-toi d'avoir :  
✅ **Docker** et **Docker Compose** installés sur ton serveur  
✅ Un accès SSH à ton serveur  
✅ Un dossier dédié pour Dashy  

#### 🔹 **2. Télécharger et Configurer**  
1. **Créer le répertoire Dashy**  
   ```bash
   mkdir -p /volume1/docker/dashy
   cd /volume1/docker/dashy
   ```

2. **Créer le fichier `docker-compose.yml`**  
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
   Exécute la commande suivante pour lancer le conteneur :  
   ```bash
   docker-compose up -d
   ```

4. **Accéder à Dashy**  
   ➡️ **http://IP-DU-SERVEUR:7444**  

---

### ⚙️ **Configuration et Personnalisation**  

#### 📝 **Modifier la configuration**  
- La configuration principale se trouve dans le fichier :  
  ```bash
  /volume1/docker/dashy/dashyconf.yml
  ```
- Après modification, redémarrer Dashy :  
  ```bash
  docker-compose restart
  ```

#### 🖼 **Gestion des icônes**  
- Les icônes personnalisées peuvent être placées dans :  
  ```bash
  /volume1/docker/dashy/icons
  ```

---

### 🔒 **Sécurité et Accès à Distance**  

Si tu veux accéder à Dashy depuis Internet :  
- Configure un **reverse proxy** avec **NGINX** ou **Traefik**  
- Active une **authentification** pour protéger l’accès  
- Ajoute un certificat **SSL/TLS** avec **Let's Encrypt**  

---

### 🚀 **Gestion du Conteneur**  

| Commande | Action |
|----------|--------|
| `docker-compose up -d` | Démarrer le conteneur |
| `docker-compose down` | Arrêter et supprimer le conteneur |
| `docker-compose restart` | Redémarrer le conteneur |
| `docker logs dashy --tail=50 -f` | Voir les logs en temps réel |
| `docker ps` | Vérifier si le conteneur est actif |

---

### 🛠 **Dépannage**  

Si Dashy ne démarre pas :  
1️⃣ Vérifier les logs :  
   ```bash
   docker logs dashy
   ```  
2️⃣ Vérifier si les ports sont libres :  
   ```bash
   netstat -tulnp | grep 7444
   ```  
3️⃣ Essayer de relancer le conteneur :  
   ```bash
   docker-compose down && docker-compose up -d
   ```  

---

### 🐝 **Sources et Documentation**  
📚 **Docs Officielles** : [https://dashy.to/docs](https://dashy.to/docs)  
🐙 **Repo GitHub** : [https://github.com/Lissy93/dashy](https://github.com/Lissy93/dashy)  

---

🎉 **Félicitations !** Tu as maintenant **Dashy** opérationnel 🚀.

### 🐝 **Execution du Playbook**
   ```bash
    ansible-playbook main.yml -i production             
   ```     