# class2-session-2025

# Comprendre les Sessions en PHP 8

## 1. Qu'est-ce qu'une session PHP ?

Sur le web, le protocole HTTP est "sans état" (stateless). Cela signifie que chaque requête d'un client (navigateur) vers un serveur est indépendante. Le serveur ne se souvient pas des requêtes précédentes de ce même client.

Les **sessions** sont un mécanisme permettant de conserver des informations sur un utilisateur spécifique à travers plusieurs requêtes et plusieurs pages. Elles permettent de "simuler" un état et de personnaliser l'expérience utilisateur.

**Pourquoi utiliser les sessions ?**

* **Systèmes de connexion :** Se souvenir qu'un utilisateur est connecté.
* **Paniers d'achat :** Garder en mémoire les articles qu'un utilisateur souhaite acheter.
* **Préférences utilisateur :** Mémoriser des choix comme la langue, le thème d'un site.
* **Données temporaires :** Stocker des messages de statut (ex: "Article ajouté avec succès !") entre les pages.
* **Droits :** Donner des droits différents suivant l'utilisateur connecté

## 2. Comment fonctionnent les sessions ?

Le mécanisme de session implique généralement :

1.  **Un identifiant de session (Session ID) :**
    * Lorsqu'une session est démarrée pour un utilisateur, PHP génère un identifiant unique (une longue chaîne de caractères aléatoires).
    * Cet ID est la clé qui permet de retrouver les données de cet utilisateur spécifique.

2.  **Transmission de l'ID de session :**
    * Le plus souvent, cet ID est stocké dans un **cookie** sur le navigateur de l'utilisateur. À chaque requête, le navigateur renvoie automatiquement ce cookie au serveur.
    * Si les cookies sont désactivés, PHP peut (mais ce n'est plus recommandé pour des raisons de sécurité et de SEO) passer l'ID de session dans l'URL (ex: `mapage.php?PHPSESSID=abcdef123456`).

3.  **Stockage des données côté serveur :**
    * Les données de la session (par exemple, le nom d'utilisateur, les articles du panier) ne sont PAS stockées sur le client (navigateur). Elles sont stockées sur le **serveur**.
    * L'ID de session permet au serveur de retrouver le bon "fichier" ou enregistrement de données correspondant à l'utilisateur. Par défaut, PHP stocke ces données dans des fichiers temporaires sur le serveur (dans un répertoire défini par `session.save_path` dans `php.ini`), dans `Wamp` c'est dans le dossier `tmp`.

## Ouvrir une session
Pour ouvrir une session en PHP, il faut utiliser la fonction `session_start()`. Cette fonction doit être appelée avant tout code HTML.

```php
<?php
session_start();
?>
<!DOCTYPE html>
<html>
...
```

Si la session n'est pas ouverte, PHP ne pourra pas accéder aux variables de session.

Si une session est déjà ouverte, `session_start()` ne fait rien que transmettre la clé de session au client.

## Fermer une session
Pour fermer une session, il suffit d'appeler la fonction `session_destroy()`. Cette fonction détruit toutes les données de la session en cours.

```php
<?php
// Initialisation de la session.
// Si vous utilisez un autre nom
// session_name("autrenom")
session_start();

// Détruit toutes les variables de session
$_SESSION = array();

// Si vous voulez détruire complètement la session,
// effacez également
// le cookie de session.
// Note : cela détruira la session et pas seulement
// les données de session !
if (ini_get("session.use_cookies")) {
    $params = session_get_cookie_params();
    setcookie(session_name(), '', time() - 42000,
        $params["path"], $params["domain"],
        $params["secure"], $params["httponly"]
    );
}

// Finalement, on détruit la session.
session_destroy();
?>
```
