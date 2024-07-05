Description
Dans le cadre de cette exercice, j'ai provisionné une VM sur GCP en utilisant Terraform, puis déployé une application Flask sur cette VM en utilisant Ansible. L'application est servie via Gunicorn et proxyée par Nginx pour des performances et une sécurité accrues.

Prérequis:
Terraform
Ansible
Google Cloud SDK 
Un compte GCP avec les permissions nécessaires pour créer des ressources.
Python

Étapes de Configuration:
1. Configuration de Terraform
Initialisation de Terraform :

J'ai ouvert un terminal et initialisé Terraform dans le répertoire contenant les fichiers de configuration Terraform :

terraform init

Création du fichier terraform.tfvars :

J'ai créé un fichier nommé terraform.tfvars et renseigné les informations spécifiques à mon projet GCP, telles que l'ID du projet, la région, la zone, et l'email du compte de service. Par exemple :

project = "your-gcp-project-id"
region = "europe-west3"
zone = "europe-west3-a"
service_account_email = "service-account-name@your-project-id.iam.gserviceaccount.com"

Application de la configuration Terraform :

J'ai exécuté la commande suivante pour appliquer la configuration Terraform et provisionner les ressources sur GCP :

terraform apply

Cette commande a créé une VM avec une adresse IP publique, un réseau VPC et une règle de pare-feu permettant le trafic HTTP.

Récupération de l'adresse IP publique :

Une fois la configuration appliquée, j'ai noté l'adresse IP publique de la VM affichée dans les sorties de Terraform. Cette adresse sera utilisée pour accéder à la VM et y déployer l'application.

2. Déploiement de l'Application avec Ansible
Mise à jour du fichier d'inventaire Ansible :

J'ai ouvert le fichier d'inventaire Ansible et ajouté l'adresse IP publique de la VM. Ce fichier indique à Ansible où se trouvent les serveurs à configurer. Par exemple :

ini

[all]
vm ansible_host=<vm_public_ip> ansible_user=<vm_ssh_user>
J'ai remplacé <vm_public_ip> par l'adresse IP publique de la VM et <vm_ssh_user> par le nom d'utilisateur SSH (souvent ubuntu ou debian).

Exécution du playbook Ansible :

J'ai exécuté le playbook Ansible pour configurer la VM et déployer l'application Flask :

ansible-playbook -i inventory playbook.yml
Ce playbook a réalisé les actions suivantes :

Mise à jour du système et installation de Python3, pip et virtualenv.
Création d'un utilisateur dédié pour exécuter l'application.
Copie du code de l'application sur la VM.
Configuration d'un environnement virtuel Python et installation des dépendances.
Configuration de Gunicorn pour servir l'application Flask.
Configuration d'un service systemd pour s'assurer que l'application démarre automatiquement au démarrage de la VM.
Installation et configuration de Nginx pour proxyer les requêtes vers Gunicorn.
Accéder à l'Application
Une fois le déploiement terminé, j'ai ouvert mon navigateur et accédé à l'adresse IP publique de la VM. J'ai vu le message Hello, World! affiché par l'application Flask.

Nettoyage
Pour supprimer toutes les ressources créées par Terraform, j'ai exécuté la commande suivante :

terraform destroy

Conclusion
Ce projet montre comment utiliser Terraform et Ansible ensemble pour provisionner des ressources cloud et déployer des applications de manière automatisée. En suivant ces étapes, j'ai pu facilement déployer et gérer une application sur GCP.