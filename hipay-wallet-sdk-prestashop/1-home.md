# Guide de configuration du module HiPay Direct pour PrestaShop


#Introduction

Dans ce document, nous décrivons la méthode pour activer et utiliser le module Hipay pour PrestaShop. Nous listons également les différents points de blocages qui peuvent survenir ainsi que leurs solutions.

**A savoir :**

-   Le module Hipay est présent en natif sur le CMS PrestaShop. Il n’est
    donc pas nécessaire de le télécharger à part, il suffit de l’activer
    sur votre backoffice PrestaShop, dans la rubrique « Modules »

-   Il est possible de télécharger le module en cas de réinstallation ou
    de mise à jour à cette adresse :
    [*http://addons.prestashop.com/en/payments-gateways-prestashop-modules/1746-hipay.html*](http://addons.prestashop.com/en/payments-gateways-prestashop-modules/1746-hipay.html)

-   Le module gère uniquement le paiement à l’acte. Hipay propose
    également un service d’abonnement, il faut dans ce cas nous
    contacter afin que l’on vous fournisse la documentation technique
    pour le développer.

-   Si vous souhaitez configurer plusieurs devises, vous devez ouvrir
    autant de sous-compte sur votre compte Hipay. Pour cela, cliquez sur
    « Synthèse des comptes » et « Créer un compte secondaire ».
    Remplissez le formulaire et choisissez la devise désirée.
    Attention : il faut inscrire votre site sur chaque sous-compte.

-   Le module gère uniquement notre plateforme de production. Vous ne
    pouvez donc pas utiliser vos identifiants de la plateforme de test.

#Pré-requis

##Inscription d’un compte marchand

Pour utiliser Hipay, vous avez besoin de posséder un compte marchand. C’est un processus simple qui se fait directement en ligne. Rendez vous sur [*www.hipay.com*](http://www.hipay.com), dans le cadre « Vous êtes Marchand » cliquez sur « Ouvrir un compte » et suivez les instructions. Une fois que vous aurez créé votre compte Hipay, vous recevrez une confirmation par email avec les instructions pour finaliser votre inscription.

![HiPay Direct Dashboard](images/media/dashboard.png) 

#Inscrivez votre site internet

Inscrivez votre site internet dans votre compte marchand

- Cliquez sur “Création de bouton”: https://www.hipay.com/website et cliquez sur "Enregistrer un nouveau site".

Les informations demandées sont utilisés pour distinguer les
différents sites internet enregistrés dans votre compte.

Entrez le nom de votre site, l’URL, le thème principal et secondaire,le mot de passe marchand, l’email de contact et le téléphone
(optionnel).

![HiPay Direct edit your website](images/media/edit_website.png)

Vous pouvez également insérer un CSS (Cascading Style Sheet) pour
personnaliser votre page de paiement Hipay (elle doit être validée par nos techniciens avant son activation).

Il n’est pas nécessaire de créer un bouton de paiement pour utiliser Hipay sur PrestaShop. Vous pouvez donc revenir sur la liste de vos sites une fois votre site inscrit.
  

#Installation et configuration

##Configuration du compte

Une fois votre site inscrit, rendez-vous sur votre backoffice
PrestaShop.

Cliquez sur « Modules » et sur « Paiement ». Au niveau du module Hipay,cliquez sur « Installer » :

![](images/media/image7.png)

Cliquez ensuite sur « Configurer »

![](images/media/image8.png)

![](images/media/module1.png)

![](images/media/module2.png)
![](images/media/module3.png)

Le **numéro de compte** se trouve en haut à droite sur votre compte
Hipay

![legend](images/media/account_id.png)

Le **mot de passe marchand** est le mot de passe que vous avez choisi lors de l’inscription de votre site.
Si vous l’avez oublié, vous pouvez le renouveler en vous connectant sur votre compte Hipay, en cliquant sur « Création de bouton » et « Editer » à coté de votre site.

![legend](images/media/edit_website.png)

L’**id du site** est l’id de votre site inscrit sur Hipay. Vous pouvez le trouver en cliquant sur « Création de bouton »

![legend](images/media/list_websites.png)


#Choix du bouton de paiement

Choisissez le bouton de paiement que vous souhaitez voir apparaître sur votre page de commande et cliquez sur « Enregistrer les modifications »

![](images/media/module4.png)

#Choix des zones

Choisissez les zones dans lesquelles vous souhaitez proposer Hipay et validez.

![](images/media/module5.png)


#Points de blocages

##Configuration du compte

> « Impossible de récupérer les catégories Hipay. Merci de vous référer
> à votre journal des erreurs pour plus de détails. »

-   Vérifiez votre id site, le système ne le reconnaît ou ne le
    trouve pas.

> « Les catégories Hipay ne sont pas définies pour chaque ID de site. »

-   Vous n’avez pas sélectionné la catégorie pour le compte dans le
    menu déroulant.

##Lors du paiement

> « Il y a 1 erreur :\[Hipay\] MerchantAccount or merchantUserSpace does
> not exist or disabled - Invalid login value : 0000000
> errorMerchantAccount or merchantUserSpace does not exist or disabled -
> Invalid login value : 0000000 ) »

-   Vérifiez votre id compte, le système ne le reconnaît ou ne le trouve
    pas.\
    Si l’identifiant est correcte, contactez notre service client à
    [*contact@hipay.com*](mailto:contact@hipay.com) pour connaître la
    raison de sa désactivation.

> « \[Hipay\] Invalid orderCategory value : 000 errorInvalid
> orderCategory value : 000 ) »

-   L’id de la catégorie sélectionné n’est pas correcte. Peut arriver
    suite à une modification des catégories sur votre compte Hipay.
    Recommencez la configuration sur votre backoffice PrestaShop pour
    obtenir ou renouveler les catégories. Si deux catégories « Autres »
    apparaissent, choisissez la deuxième et validez.

> « \[Hipay\] Invalid merchant website password ! ( error Invalid
> merchant website password ! ) »

-   Le mot de passe marchand que vous avez indiqué n’est pas correct. Le
    mot de passe marchand est le mot de passe que vous avez choisi lors
    de l’inscription de votre site.\
    Si vous l’avez oublié, vous pouvez le renouveler en vous connectant
    sur votre compte Hipay, en cliquant sur « Création de bouton » et «
    Editer » à coté de votre site.

> «  Le paiement par Hipay n’est pas proposé sur la page de paiement ? »

-   Suivez cette manipulation :\
    Configurez vos informations de connexion sur le module, validez,
    retournez sur la liste des modules et réinitialisez le module Hipay,
    retournez sur la configuration du module, définissez les zones
    et validez. Le paiement par Hipay devrait être proposé.

#Support

En cas de question concernant les reversements et votre contrat, envoyez un email à [*prestashop@hipay.com*](mailto:prestashop@hipay.com)

En cas de question technique, contactez notre support technique à [*technique@hipay.com*](mailto:technique@hipay.com)
