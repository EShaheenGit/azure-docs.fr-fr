---
title: "FAQ de l’application web Azure App Service sur Linux | Microsoft Docs"
description: "FAQ de l’application web Azure App Service sur Linux."
keywords: azure app service, application web, faq, linux, oss
services: app-service
documentationCenter: 
authors: ahmedelnably
manager: erikre
editor: 
ms.assetid: 
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: aelnably;wesmc
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: dcce8d855f8c37d40fe8f09ef0a97e46b342e3cf
ms.contentlocale: fr-fr
ms.lasthandoff: 05/10/2017


---

# <a name="azure-app-service-web-app-on-linux-faq"></a>FAQ de l’application web Azure App Service sur Linux

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]


Avec la mise en production de l’application web sur Linux, nous travaillons à l’ajout de fonctionnalités et à l’amélioration de notre plateforme. Voici quelques-unes des questions courantes que nos clients nous ont posées au cours des derniers mois.
Si vous avez une question, commentez l’article ; nous vous répondrons dès que possible.

## <a name="built-in-images"></a>Images prédéfinies

**Q :** Je veux répliquer les conteneurs Docker prédéfinis fournis par la plateforme. Où se trouvent ces fichiers ?

**R :** vous trouverez tous les fichiers Docker sur [GitHub](https://github.com/azure-app-service). Vous trouverez tous les conteneurs Docker sur [Docker Hub](https://hub.docker.com/u/appsvc/).

**Q :** Quelles sont les valeurs attendues de la section Fichier de démarrage lorsque je configure la pile d’exécution ?

**R :** Pour Node.Js, vous spécifiez le fichier de configuration PM2 ou votre fichier de script. Pour .NET Core, vous spécifiez le nom de votre DLL compilée. Pour Ruby, vous pouvez spécifier le script Ruby avec lequel initialiser votre application.

## <a name="management"></a>Gestion

**Q :** Que se passe-t-il lorsque j’appuie sur le bouton Redémarrer dans le portail Azure ?

**R :** Cela revient à redémarrer Docker.

**Q :** Puis-je utiliser Secure Shell (SSH) pour me connecter à la machine virtuelle du conteneur d’application ?

**R :** Oui, vous pouvez le faire via le site SCM,. Consultez l’article suivant pour en savoir plus sur [la prise en charge SSH pour l’application Web sur Linux](./app-service-linux-ssh-support.md)

## <a name="continuous-integrationdeployment"></a>Intégration/Déploiement en continu

**Q :** Mon application web utilise toujours une ancienne image de conteneur Docker après la mise à jour de l’image sur DockerHub. Prenez-vous en charge l’intégration continue / le déploiement continu de conteneurs personnalisés ?

**R :** Pour configurer l’intégration/le déploiement en continu des images DockerHub, consultez l’article [Docker Hub Continuous Deployment with Web App on Linux](./app-service-linux-ci-cd.md) (Déploiement continu DockerHub avec l’application web sur Linux). Pour les registres privés, vous pouvez actualiser le conteneur en arrêtant puis démarrant votre application web. Vous pouvez également modifier ou ajouter un paramètre d’application factice pour forcer une actualisation de votre conteneur.

**Q :** Prenez-vous en charge les environnements intermédiaires ?

**R :** Oui.

## <a name="language-support"></a>Support multilingue

**Q :** Prenez-vous en charge les applications .NET Core non compilées ?

**R :** Oui.

**Q :** Prenez-vous en charge Composer en tant que gestionnaire de dépendances pour les applications PHP ?

**R :** Non. Vous devez déployer vos applications PHP avec toutes les dépendances. Nous envisageons la création d’une expérience de déploiement complète dans une version ultérieure.

## <a name="custom-containers"></a>Conteneurs personnalisés

**Q :** J’utilise mon propre conteneur personnalisé. Mon application se trouve dans le répertoire `\home\`, mais je ne trouve pas mes fichiers lorsque je parcours le contenu avec le [site SCM](https://github.com/projectkudu/kudu) ou un client FTP. Où sont mes fichiers ?

**R :** Nous montons un partage SMB dans le répertoire `\home\`. Cela remplace tout le contenu qui s’y trouve.

**Q :** Quel est le format de l’URL du serveur de registre privé ?

**R :** Vous devez fournir l’URL de registre complète, y compris `http://` ou `https://`.

**Q :** Quel est le format du nom d’image dans l’option de registre privé ?

**R :** Vous devez ajouter le nom d’image complet, y compris l’URL de registre privé (par exemple : myacr.azurecr.io/dotnet:latest)

**Q :** Je veux exposer plusieurs ports sur l’image de mon conteneur personnalisé. Est-ce possible ?

**R :** Cela n’est actuellement pas pris en charge.

**Q :** Puis-je apporter mon propre système de stockage ?

**R :** Cela n’est pas actuellement pris en charge.

**Q :** Je n’arrive pas à parcourir le système de fichiers de mon conteneur personnalisé à partir du site SCM. Pourquoi ?

**R :** Le site SCM s’exécute dans un conteneur distinct. Vous ne pouvez pas vérifier le système de fichiers ou les processus en cours d’exécution du conteneur d’application.

**Q :** Mon conteneur personnalisé écoute un autre port que le port 80. Comment puis-je configurer mon application pour acheminer les demandes vers ce port ?

**R :** La détection automatique du port est activée, mais vous pouvez aussi spécifier un paramètre d’application appelé **PORT** et lui attribuer la valeur du numéro de port attendu.

**Q :** Dois-je implémenter HTTPS dans mon conteneur personnalisé ?

**R :** Non, la plateforme gère l’annulation HTTPS au niveau des serveurs frontaux partagés.

## <a name="pricing-and-sla"></a>Tarifs et contrat SLA

**Q :** Quelle est la tarification pour l’utilisation de la préversion publique ?

**R :** La moitié du nombre d’heures d’exécution de votre application vous est facturée, à la tarification Azure App Service normale. Cela signifie que vous bénéficiez d’une remise de 50 pour cent sur la tarification normale d’Azure App Service.

## <a name="other"></a>Autres

**Q :** Quels sont les caractères pris en charge dans les noms de paramètres d’application ?

**R :** Vous ne pouvez utiliser que les caractères A-Z, a-z, 0-9 et le trait de soulignement pour les paramètres d’application.

**Q :** Où puis-je demander de nouvelles fonctionnalités ?

**R :** Vous pouvez proposer votre idée sur le [Forum de commentaires pour Web Apps](https://aka.ms/webapps-uservoice). Ajoutez « [Linux] » au titre de votre idée.

## <a name="next-steps"></a>Étapes suivantes
* [Qu’est-ce qu’Azure Web App sur Linux ?](app-service-linux-intro.md)
* [Création d’applications web dans l’application web Azure sur Linux](app-service-linux-how-to-create-web-app.md)
* [Prise en charge SSH pour l’application web Azure sur Linux](./app-service-linux-ssh-support.md)
* [Configurer des environnements intermédiaires dans Azure App Service](./web-sites-staged-publishing.md)
* [Déploiement continu Docker Hub avec l’application web Azure sur Linux](./app-service-linux-ci-cd.md)

