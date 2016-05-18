![alt text][logo]

[logo]: https://github.com/hipay/hipay-docs/blob/develop/images/header.jpg "logo header"

# Pos Payment HiPay

## Introduction
Le paiement avec un terminal électronique s'introduit dans le e-commerce pour ajouter un parcours utilisateur alliant Internet et les points de vente.
Aujourd'hui, il y a deux tendances comme le Web-to-Store et le Store-to-Web.

Le Web-to-Store propose de procéder un achat en ligne avec le paiement directement en point de vente, le but est de faire revenir les clients en boutique afin d'optimiser le cross-selling ou le up-selling.

Le Store-to-Web est différent que la précédente méthode car celle-ci propose aux clients de commander directement dans le point de vente via une borne avec tablette et un TPE. Le client souhaite un produit mais qui n'est plus en stock dans le point de vente, alors l'achat via une borne permettra de valider une commande puis le client sera livré à domicile.
Ainsi le client ne repart pas de la boutique les mains vides.
Ci-dessous, nous vous décrivons comment intégrer notre API le plus simplement.

## Objectif
HiPay Fullservice met à disposition une API permettant d'interfacer un site marchand avec des terminaux de paiement électronique. Ce document a pour objectif d'aider l'intégration de cette méthode de paiement.

Pour utiliser cette API, vous devez au préalable obtenir une Apikey auprès de HiPay.

------

## Sommaire

### 1 Introduction
##### 1.1 Besoin fonctionnel
##### 1.2 Différents acteurs
##### 1.3 Ressources techniques
### 2 Profil Web-to-Store
##### 2.1 Architecture technique
##### 2.2 Diagramme de séquence
##### 2.3 Mockups Front Office
##### 2.4 Description technique
### 3 Profil Store-to-Web
##### 3.1 Architecture technique
##### 3.2 Diagramme de séquence
##### 3.3 Mockups Front Office
##### 3.4 Description technique
### 4 Configuration Magento Back office
##### 4.1 Mockups Point de vente
##### 4.2 Mockups TPE
##### 4.3 Mockups Methode de paiement
##### 4.4 Description technique
### 5 Webservice - API CISS
##### 5.1 Tableau de données à envoyer pour la transaction
##### 5.2 Identification avec la méthode cURL
##### 5.3 Exemple requête / réponse avec l'API
##### 5.4 Règles sur les transaction en attente de paiement

------

## 1 - Introduction

### 1.1 - Besoin fonctionnel

Aujourd'hui, il y a deux tendances comme le Web-to-Store et le Store-to-Web.
Le Web-to-Store propose de procéder un achat en ligne avec le paiement directement en point de vente, le but est de faire revenir les clients en boutique afin d'optimiser le cross-selling ou le up-selling.

Le Store-to-Web est différent que la précédente méthode car celle-ci propose aux clients de commander directement dans le point de vente via une borne avec tablette et un TPE. Le client souhaite un produit mais qui n'est plus en stock dans le point de vente, alors l'achat via une borne permettra de valider une commande puis le client sera livré à domicile.
Ainsi le client ne repart pas de la boutique les mains vides.

Le CMS ou SDK devra, au moment de l’achat du client, proposer une méthode de paiement en point de vente. C-à-d, le client au moment de la confirmation de commande, le site proposera de payer sur un terminal de paiement électronique. Le client devra insérer sa carte et saisir son code PIN pour procéder à une transaction.

En cas de succès et d’échec, le CMS ou SDK devra retourner une page de succès ou une page “Failure” expliquant la raison de l’échec.

### 1.2 - Différents acteurs

Voici les différents acteurs touchant au système en front office et back office :

1. Le marchand : mise en place du module dans son CMS et configuration du module avec son contrat VAD
2. Le client : Celui qui passe commande et qui utilisera le TPE avec sa carte et son PIN
3. HiPay : L'établissement de paiement faisant la relation entre le TPE, le client et le marchand
4. Partenaire : L'organisme permettant de faciliter la communication entre les TPE et les CMS (CISS est notre partenaire officiel)
5. Acquéreur : L'organisme permettant de faire de la réconsiliation

### 1.3 - Ressources techniques

Pour que chaque fonctionnalité marche, il faut des pré-requis :

1. Être à jour avec le CMS / framework utilisé 
2. cURL installé sur l'hébergement
3. Avoir les permissions d'accès dans les deux sens entre le CMS et l'API (callback par exemple)
4. Un terminale de paiement en mode test ou avec un contrat VAD
5. une borne permettant de passer commande ou un site e-commerce
6. Vos identifiants API (apiKey, login, password, ...)
7. Le module HiPay installé sur le CMS et configuré avec les informations ci-dessus

------

## 2 - Profil Web-to-Store

Le parcours utilisateur du Web-to-Store est une commande effectuée sur le site e-commerce du marchand mais dans le domicile du client (ou autre mais en dehors d'un point de vente). 
Le client navigue donc sur le site et valide sa commande en choisissant le paiement en point de vente. 
Dans l'étape de la livraison, le client a choisit un point de retrait dans le point de vente en question.
Lorsque sa commande est prête sur le lieu du point de vente, alors le client se rend dans celui-ci et procède à un paiement sur le terminal de paiement électronique. La commande est terminée.

### 2.1 - Architecture technique

![alt text][img21]

[img21]: https://github.com/hipay/hipay-docs/blob/develop/images/dat-web-to-store.png "Architecture technique Web-to-Store"

### 2.2 - Diagramme de séquence

### 2.3 - Mockups Front Office

![alt text][img23a]

[img23a]: https://github.com/hipay/hipay-docs/blob/develop/images/dat-store-to-web.png "Architecture technique Store-to-Web"

### 2.4 - Description technique

------

## 3 - Profil Web-to-Store

Le parcours utilisateur du Store-to-Web est une commande effectuée sur place dans un point de vente. 
Le client navigue sur une borne ayant accès sur le site e-commerce.
Le client valide sa commande en choisissant le paiement en point de vente. Pour information, le client procède à une livraison de son choix. Une page d'attente de paiement s'affiche et indique au client de payer par CB sur le TPE physique dans la boutique. La commande est terminée.

### 3.1 - Architecture technique

![alt text][img31]

[img31]: https://github.com/hipay/hipay-docs/blob/develop/images/dat-store-to-web.png "Architecture technique Store-to-Web"

### 3.2 - Diagramme de séquence

### 3.3 - Mockups Front Office

### 3.4 - Description technique

------
