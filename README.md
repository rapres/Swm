# Swm
Projet Sécurité Web Mail 
Ce projet est intitulé de intégration Openssl dans la messagerie de mozilla thunderbird 

Description

Ce projet consiste à mettre en place une messagerie sécurisée en environnement local en utilisant :

- Postfix (SMTP)
- Dovecot (IMAP)
- OpenSSL
- Mozilla Thunderbird
- S/MIME

Le système permet l’échange sécurisé de messages entre utilisateurs grâce au chiffrement asymétrique et aux certificats numériques.

---

Objectifs du projet

- Configurer un serveur de messagerie local;
- Permettre l’envoi et la réception des emails;
- Générer des certificats numériques;
- Implémenter le chiffrement S/MIME;
- Signer numériquement les messages;
- Garantir :
  - la confidentialité;
  - l’authenticité;
  - l’intégrité des messages.

---

Architecture du projet

Le projet est composé de 3 machines virtuelles :

1. Machine CA (Certificate Authority)

Rôle :
- Génération des certificats
- Signature des certificats utilisateurs
- Gestion de la confiance

Fonctionnalités :
- Création de `ca.crt`
- Génération des clés privées/publiques
- Signature des certificats Etudiant et Professor

---

2. Machine Serveur

Rôle :
- Transport et stockage des emails

Services installés :
- Postfix pour le SMTP(envoie)
- Dovecot pour le IMAP(réception)

Fonctionnalités :
- Envoi des mails;
- Réception des mails;
- Gestion des boîtes Maildir;
- Authentification des utilisateurs.

---

3. Machine Client

Rôle :
- Utilisation de la messagerie

Logiciel utilisé :
- Mozilla Thunderbird

Fonctionnalités :
- Envoi et réception des emails;
- Signature numérique;
- Chiffrement S/MIME;
- Déchiffrement des messages.

---

Sécurité implémentée

Le projet utilise le chiffrement asymétrique :

- Clé publique pour le  chiffrement
- Clé privée pour le déchiffrement

Technologie utilisée :
- S/MIME

Fonctionnalités de sécurité :
- Confidentialité;
- Authentification;
- Intégrité.

---

Technologies utilisées

| Technologie   | Rôle           |
|---------------|----------------|
| Ubuntu Server | Système        |
| Postfix       | SMTP           |
| Dovecot       | IMAP           |
| OpenSSL       | Certificats    |
| Thunderbird   | Client mail    |
| S/MIME        | Sécurité email |

---------------

Structure du projet


Installation et configuration
Installation des services
Postfix:
sudo apt install postfix

Dovecot:
sudo apt install dovecot-imapd dovecot-pop3d

Thunderbird:
sudo apt install thunderbird

Génération des certificats
Création de la CA:
openssl genrsa -out ca.key 2048

openssl req -x509 -new -nodes -key ca.key -sha256 -days


Tests réalisés

Test SMTP

Connexion au serveur SMTP :
telnet 192.168.56.20 25

Résultat attendu :
220 mail.local ESMTP Postfix

Test IMAP
Connexion au serveur IMAP :
telnet 192.168.56.20 143

Résultat attendu :
* OK [CAPABILITY IMAP4rev1] Dovecot ready

Test d’authentification IMAP
a login Professor 1234 où 1234 est le mot de passe

Résultat :
a OK Logged in

Fonctionnement du chiffrement S/MIME

Envoi du message
Étudiant chiffre le message avec la clé publique de Professor 
Le message est signé avec la clé privée d' Étudiant 

Réception du message
Professor utilise sa clé privée pour déchiffrer le message
Thunderbird vérifie automatiquement la signature

Ports utilisés
|Service           | Port  |
|------------------|-------|
|SMTP (Postfix)    | 25    |
|IMAP (Dovecot)    | 143   |
|HTTPS (Apache SSL)| 443   |
___________________________

Commandes importantes
Vérifier Postfix:
sudo systemctl status postfix

Vérifier Dovecot:
sudo systemctl status dovecot

Vérifier les ports:
sudo ss -tlnp

Voir les logs mails:
sudo tail -f /var/log/mail.log

Maildir
Les emails sont stockés dans :
/home/professor/Maildir/
/home/etudiant/Maildir/

Structure :
Maildir/
├── cur
├── new
└── tmp

Limitations du projet
Projet exécuté uniquement en réseau local;
Pas de DNS public;
Pas de nom de domaine réel;
Pas de configuration SPF/DKIM/DMARC;
Pas de certificat public reconnu par Internet.

Améliorations possibles
Ajouter TLS/SSL pour SMTP et IMAP;
Utiliser un vrai nom de domaine;
Déployer sur un serveur réel;
Ajouter SPF, DKIM et DMARC;
Configurer HTTPS avec certificat public;
Ajouter une interface webmail.

Diagramme simplifié
+-------------+
|     CA      |
| Certificats |
+-------------+
       |
       v
+-------------+
|   Serveur   |
| Postfix     |
| Dovecot     |
+-------------+
       |
       v
+-------------+
|   Client    |
| Thunderbird |
+-------------+

Présentation du scénario:
Étudiant:
Rédige un email;
Signe et chiffre le message;
Envoie le mail.

Professeur:
Reçoit le mail;
Déchiffre le message;
Vérifie la signature.

Sécurité apportée
Confidentialité: Seul le destinataire peut lire le message.
Intégrité: Le message n’a pas été modifié.
Authentification: L’identité de l’expéditeur est vérifiée.

Références
OpenSSL Documentation
Postfix Documentation
Dovecot Documentation
Mozilla Thunderbird Documentation