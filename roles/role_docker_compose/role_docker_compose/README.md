# Ansible Role: docker_compose

## 📌 Description
Ce rôle Ansible installe et gère Docker ainsi que Docker Compose sur un serveur distant. Il assure que Docker est bien installé, récupère Docker Compose si nécessaire et permet de démarrer des services avec `docker-compose`.

## 📁 Structure du Rôle
```
roles/
└── docker_compose/
    ├── README.md  # Documentation du rôle
    ├── defaults/
    │   └── main.yml  # Variables modifiables par le playbook
    ├── tasks/
    │   ├── main.yml  # Tâches principales du rôle
    │   ├── install.yml  # Installation de Docker et Docker Compose
    │   ├── start.yml  # Lancement du service Docker Compose
    ├── handlers/
    │   └── main.yml  # Gestion des notifications (restart)
    ├── meta/
    │   └── main.yml  # Métadonnées du rôle
```

## 🎯 Fonctionnalités
- Vérifie si Docker est installé, sinon l'installe.
- Vérifie si Docker Compose est installé, sinon l'installe.
- Gère le lancement de services via Docker Compose.
- Fournit un point d'entrée simple pour l'utilisation dans un playbook Ansible.

## ⚙️ Variables
Les variables du rôle peuvent être modifiées dans le playbook principal si nécessaire.

### 🔹 Variables par défaut (`defaults/main.yml`)
```yaml
---
docker_compose_bin: "/usr/local/bin/docker-compose"
docker_path: "/opt/docker"
compose_file: "{{ docker_path }}/docker-compose.yml"
```

### 🔹 Exemple de surcharge dans un playbook
Si vous utilisez ce rôle dans un projet spécifique (ex: Dashy), vous pouvez redéfinir certaines variables :
```yaml
vars:
  docker_path: "/volume1/docker/dashy"
  compose_file: "{{ docker_path }}/docker-compose.yml"
```

## 🚀 Utilisation
Ajoutez ce rôle dans votre playbook Ansible :
```yaml
- hosts: all
  become: true
  roles:
    - docker_compose
```

## 🔄 Handlers
Le rôle inclut un handler permettant de redémarrer un service Docker Compose :
```yaml
- name: Redémarrer Docker Compose
  command: "{{ docker_compose_bin }} -f {{ compose_file }} restart"
  args:
    chdir: "{{ docker_path }}"
```

## 🛠️ Dépendances
Aucune dépendance spécifique.

## 📌 Auteur
**mbenoit44** - Rôle conçu pour gérer et déployer des services via Docker Compose avec Ansible sur un NAS synology

---

Ce rôle est modulaire et peut être utilisé pour d'autres applications Docker en adaptant les variables spécifiques à chaque projet. 🚀