# CoolocDeployment
Projet de déploiement pour le projet Cooloc.

Cooloc est une application de gestion de dépense en colocation gérant les dépenses avec une fonctionnalité d'analyse de ticket de caisse.

Basé sur les projets suivants :  
- [Cooloc API](https://github.com/ComePicard/Cooloc)
- [Cooloc OCR](https://github.com/ComePicard/CoolocOCR)
- [Cooloc UI](https://github.com/lucassrz/cooloc-expense-manager)

## Rapport d'avancement

### Cooloc API :
Projet d'API codé en Python utilisant la librairie FastAPI. Les données sont stockées dans une base de données PostgreSQL.
Nous n'avons pas utilisé d'ORM pour communiquer avec notre DB car nous voulons avoir le contrôle sur nos requêtes.

Nous séparons les processus autant que possible. Par exemple à l'appel d'un endpoint, la fonction gérant cette endpoint ne doit appeler qu'un service, qui va gérer la récupération des données, les règles métiers et son formattage. Cette séparation s'observe dans la nomenclature des fonctions :
-  Les endpoints utilisent les verbes HTTP (GET, POST, PATCH) suivi de l'action (get_documents_by_id).
-  Les services utilisent des termes techniques tels que fetch la récupération, create pour la création, edit pour la mise à jour (fetch_document_by_id).
-  Les DAO utilisent les mots clées SQL (SELECT, INSERT, UPDATE) suivi de l'action (select_document_by_id).
Pour résumer voici un paint décrivant la pipeline :
![image](https://github.com/user-attachments/assets/cdd7b512-10d7-4545-9a34-6f2b6dcb6b0d)

Les schéma de données sont définis dans le répertoire /app/schema, chaque objet possède un schéma basique et un create. Le Create est un modèle sans les IDs et les champs updated_at et deleted_at, il est destiné à la création de l'objet.
Toutes les contraintes des champs décrits dans ces schémas sont répercutés dans le contrat d'API openAPI.

### Cooloc OCR :
Projet d'OCR pour le projet Cooloc.
Utilise l'API OpenAI pour l'extraction du texte du ticket de caisse ainsi que pour l'analyse de ce texte.
Utilisation de pydantic pour la validation du format de l'objet de retour.

Nous avons d'abord essayé d'utiliser des modèles installés en local (MiniLM et LLaVA). Mais ces derniers étaient soit trop complexe (besoin d'un jeu d'entraînement), soit pas suffisamment fiable. 
Nous avons ensuite testé Tesseract, un outil d'OCR. Mais ce dernier n'était pas aussi fiable que OpenAI, ce sur quoi nous nous sommes rabattu.

## Guide du développeur : 

