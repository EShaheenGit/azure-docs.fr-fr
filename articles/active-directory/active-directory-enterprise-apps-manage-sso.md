---
title: "Gestion de l’authentification unique pour les applications d’entreprise dans Azure Active Directory | Microsoft Docs"
description: "Découvrez comment gérer l’authentification unique pour les applications d’entreprise à l’aide de la version préliminaire d’Azure Active Directory"
services: active-directory
documentationcenter: 
author: asmalser
manager: femila
editor: 
ms.assetid: bcc954d3-ddbe-4ec2-96cc-3df996cbc899
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/04/2017
ms.author: asmalser
ms.translationtype: Human Translation
ms.sourcegitcommit: 9ae7e129b381d3034433e29ac1f74cb843cb5aa6
ms.openlocfilehash: 118b77064a96585b0d5b951ca56313776021624e
ms.contentlocale: fr-fr
ms.lasthandoff: 05/08/2017


---
# <a name="managing-single-sign-on-for-enterprise-apps"></a>Gestion de l’authentification unique pour les applications d’entreprise
> [!div class="op_single_selector"]
> * [Portail Azure](active-directory-enterprise-apps-manage-sso.md)
> * [portail Azure Classic](active-directory-sso-integrate-saas-apps.md)
> 

Cet article explique comment utiliser le [portail Azure](https://portal.azure.com) pour gérer les paramètres d’authentification unique pour des applications, en particulier celles qui ont été ajoutées à partir de la [galerie d’applications Azure Active Directory](active-directory-appssoaccess-whatis.md#get-started-with-the-azure-ad-application-gallery). 

## <a name="finding-your-apps"></a>Recherche de vos applications
## <a name="finding-your-apps-in-the-portal"></a>Recherche de vos applications dans le portail
Toutes les applications configurées pour une l’authentification unique dans un répertoire par un administrateur d’annuaire à l’aide de la [Galerie d’applications Azure Active Directory](active-directory-appssoaccess-whatis.md#get-started-with-the-azure-ad-application-gallery) peuvent être affichées et gérées dans le [portail Azure](https://portal.azure.com). Les applications se trouvent dans la section **Plus de services** &gt; **Applications d’entreprise** du portail. Les applications d’entreprise sont des applications qui sont déployées et utilisées au sein de votre organisation.

![Panneau Applications d’entreprise][1]

Sélectionnez **Toutes les applications** à gauche pour afficher une liste de toutes les applications qui ont été configurées, y compris les applications ajoutées depuis la galerie. La sélection d’une application charge le panneau de ressources de cette application, où vous pouvez consulter des rapports pour cette application et gérer un large éventail de paramètres.

Pour gérer les paramètres d’authentification unique, sélectionnez **Authentification unique**.

![Panneau de ressources d’application][2]

## <a name="single-sign-on-modes"></a>Modes d’authentification unique
Le panneau **Authentification unique** commence par un menu **Mode**, qui permet de configurer le mode d’authentification. Les options disponibles incluent :

* **SAML-based sign on** (Authentification basée sur SAML) : cette option est disponible si l’application prend en charge l’authentification unique fédérée complète avec Azure Active Directory à l’aide du protocole SAML 2.0.
* **Password-based sign on** (Authentification par mot de passe) : cette option est disponible si Azure AD prend en charge le remplissage de formulaire de mot de passe pour cette application.
* **Linked sign on** (Authentification liée) : précédemment appelée Authentification unique existante, cette option permet aux administrateurs de placer un lien vers cette application dans le panneau d’accès Azure AD ou le lanceur d’applications Office 365 de leurs utilisateurs.

Pour plus d’informations sur ces modes, voir [Fonctionnement de l’authentification unique avec Azure Active Directory](active-directory-appssoaccess-whatis.md#how-does-single-sign-on-with-azure-active-directory-work).

## <a name="saml-based-sign-on"></a>SAML-based sign on
L’option **Authentification basée sur SAML** affiche un panneau divisé en quatre sections :

### <a name="domains-and-urls"></a>Domains and URLs (Domaines et URL)
C’est là que toutes les informations concernant le domaine et les URL de l’application doivent être ajoutées à votre annuaire Azure AD. Toutes les entrées requises pour que l’authentification unique fonctionne sont affichées directement dans l’écran, tandis que toutes les entrées facultatives peuvent être affichées en sélectionnant la case à cocher **Show advanced URL settings** (Afficher les paramètres d’URL avancés). La liste complète des entrées prises en charge inclut :

* **URL d’authentification** : adresse où l’utilisateur se connecte à cette application. Si l’application est configurée pour effectuer une authentification unique initiée par le fournisseur de services, lorsque l’utilisateur accède à cette URL, le fournisseur de services effectue la redirection nécessaire vers Azure AD pour authentifier et connecter l’utilisateur. Si ce champ est renseigné, Azure AD utilise cette URL pour lancer l’application à partir d’Office 365 et du panneau d’accès Azure AD. Si ce champ est omis, Azure AD effectue une authentification initiée par le fournisseur d’identité lorsque l’application est lancée à partir d’Office 365, du panneau d’accès Azure AD ou à partir de l’URL de connexion unique d’Azure AD.
* **Identificateur** : cet URI doit identifier de façon unique l’application pour laquelle l’authentification unique est configurée. Il s’agit de la valeur qu’Azure AD renvoie à l’application en tant que paramètre Audience du jeton SAML. L’application doit la valider. Cette valeur apparaît également en tant qu’ID d’entité dans les métadonnées SAML fournies par l’application.
* **URL de réponse** : l’URL de réponse est là où l’application attend le jeton SAML. Elle est aussi appelée URL ACS (Assertion Consumer Service). Une fois ces entrées effectuées, cliquez sur Suivant pour passer à l’écran suivant. Cet écran fournit des informations sur ce qui doit être configuré dans l'application pour lui permettre d'accepter un jeton SAML à partir d'Azure AD.
* **Relay State** (État du relais) : l’état du relais est un paramètre facultatif permettant d’indiquer à l’application où rediriger l’utilisateur une fois l’authentification terminée. En général, la valeur est une URL valide au niveau de l’application. Cependant, certaines applications utilisent ce champ différemment (voir la documentation relative à l’authentification unique de l’application pour plus d’informations). La possibilité de définir l’état du relais est une nouvelle fonctionnalité disponible uniquement dans le nouveau portail Azure.

### <a name="user-attributes"></a>Attributs utilisateur
C’est là que les administrateurs peuvent afficher et modifier les attributs envoyés dans le jeton SAML émis par Azure AD pour l’application chaque fois qu’un utilisateur se connecte.

Le seul attribut modifiable pris en charge est l’attribut **Identificateur d’utilisateur**. La valeur de cet attribut correspond au champ d’Azure AD qui identifie de façon unique chaque utilisateur au sein de l’application. Par exemple, si l’application a été déployée en utilisant l’adresse de messagerie comme nom d’utilisateur et identificateur unique, la valeur est définie sur le champ « user.mail » d’Azure AD.

### <a name="saml-signing-certificate"></a>Certificat de signature SAML
Cette section affiche les détails du certificat utilisé par Azure AD pour signer les jetons SAML émis pour l’application chaque fois que l’utilisateur s’authentifie. Elle permet d’inspecter les propriétés du certificat actuel, notamment sa date d’expiration.

### <a name="application-configuration"></a>Configuration de l’application
La dernière section fournit la documentation ou les contrôles requis pour configurer l’application de manière à utiliser Azure Active Directory comme fournisseur d’identité.

Le menu volant **Configurer l’application** propose de nouvelles instructions incorporées concises pour la configuration de l’application. Il s’agit d’une autre nouvelle fonctionnalité disponible uniquement dans le nouveau portail Azure.

> [!NOTE]
> Pour obtenir un exemple complet de documentation incorporée, voir l’application Salesforce.com. De la documentation est ajoutée en continu pour d’autres applications.
> 
> 

![Documents incorporés][3]

## <a name="password-based-sign-on"></a>Password-based sign on
Si l’application le prend en charge, la sélection du mode d’authentification unique par mot de passe et du bouton **Enregistrer** configure instantanément ce type d’authentification pour l’application. Pour plus d’informations sur le déploiement de l’authentification unique par mot de passe, voir [Fonctionnement de l’authentification unique avec Azure Active Directory](active-directory-appssoaccess-whatis.md#how-does-single-sign-on-with-azure-active-directory-work).

![Password-based sign on][4]

## <a name="linked-sign-on"></a>Linked sign on
Si l’application le prend en charge, la sélection du mode d’authentification unique lié vous permet d’entrer l’URL de redirection à utiliser par le panneau d’accès Azure AD ou Office 365 lorsqu’un utilisateur clique sur cette application. Pour plus d’informations sur l’authentification unique liée (précédemment appelée Authentification unique existante), voir [Fonctionnement de l’authentification unique avec Azure Active Directory](active-directory-appssoaccess-whatis.md#how-does-single-sign-on-with-azure-active-directory-work).

![Authentification liée][5]

##<a name="feedback"></a>Commentaires

Nous espérons que vous appréciez votre expérience d’Azure AD améliorée. Continuez à nous faire part de vos commentaires. Publiez vos commentaires et vos idées d’amélioration dans la section du **portail Admin** de notre [forum de commentaires](https://feedback.azure.com/forums/169401-azure-active-directory/category/162510-admin-portal).  Nous sommes très heureux de créer chaque jour des nouveautés et de pouvoir bénéficier de vos commentaires pour concevoir et définir de nouvelles créations.

[1]: ./media/active-directory-enterprise-apps-manage-sso/enterprise-apps-blade.PNG
[2]: ./media/active-directory-enterprise-apps-manage-sso/enterprise-apps-sso-blade.PNG
[3]: ./media/active-directory-enterprise-apps-manage-sso/enterprise-apps-blade-embedded-docs.PNG
[4]: ./media/active-directory-enterprise-apps-manage-sso/enterprise-apps-blade-password-sso.PNG
[5]: ./media/active-directory-enterprise-apps-manage-sso/enterprise-apps-blade-linked-sso.PNG

