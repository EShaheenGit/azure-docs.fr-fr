---
title: "Didacticiel : Intégration d’Azure Active Directory à Directions on Microsoft | Microsoft Docs"
description: "Apprenez à utiliser Directions on Microsoft avec Azure Active Directory pour activer l’authentification unique, l’approvisionnement automatique et bien plus encore."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: e0c8986f-2acd-418d-a306-437abc44b640
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/10/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: efea82512dad3f2e5d1c0dd45e9e2f6db9a283d4
ms.openlocfilehash: a5538e89b1c23e4a2ad45c5515de8167ef95b46a
ms.lasthandoff: 02/17/2017


---
# <a name="tutorial-azure-active-directory-integration-with-directions-on-microsoft"></a>Didacticiel : Intégration d’Azure Active Directory à Directions on Microsoft
L’objectif de ce didacticiel est de montrer comment intégrer Azure Active Directory et Directions on Microsoft.  

Le scénario décrit dans ce didacticiel part du principe que vous disposez des éléments suivants :

* Un abonnement Azure valide
* Un abonnement Directions on Microsoft

Si vous ne disposez pas d’un abonnement fédéré à Directions on Microsoft, envoyez une demande par courrier électronique à l’adresse **service@DirectionsOnMicrosoft.com**.

À l’issue de ce didacticiel, les utilisateurs Azure Active Directory que vous avez affectés à Directions on Microsoft pourront s’authentifier dans l’application à l’aide de l’authentification unique.

Le scénario décrit dans ce didacticiel se compose des blocs de construction suivants :

* Activation de l’intégration de l’application pour Directions on Microsoft
* Configuration de l’authentification unique (SSO)
* Configuration de l'approvisionnement des utilisateurs
* Affectation d’utilisateurs

![Scénario](./media/active-directory-saas-directions-microsoft-tutorial/IC786877.png "Scénario")

## <a name="enable-the-application-integration-for-directions-on-microsoft"></a>Activer l’intégration de l’application pour Directions on Microsoft
Cette section décrit l’activation de l’intégration de l’application pour Directions on Microsoft.

**Pour activer l’application de l’intégration pour Directions on Microsoft, suivez les étapes ci-dessous :**

1. Dans le volet de navigation gauche du portail Azure Classic, cliquez sur **Active Directory**.
   
   ![Active Directory](./media/active-directory-saas-directions-microsoft-tutorial/IC700993.png "Active Directory")
2. Dans la liste **Annuaire** , sélectionnez l'annuaire pour lequel vous voulez activer l'intégration d'annuaire.
3. Pour ouvrir la vue des applications, dans la vue d'annuaire, cliquez sur **Applications** dans le menu du haut.
   
   ![Applications](./media/active-directory-saas-directions-microsoft-tutorial/IC700994.png "Applications")
4. Cliquez sur **Ajouter** en bas de la page.
   
   ![Ajouter une application](./media/active-directory-saas-directions-microsoft-tutorial/IC749321.png "Ajouter une application")
5. Dans la boîte de dialogue **Que voulez-vous faire ?**, cliquez sur **Ajouter une application à partir de la galerie**.
   
   ![Ajouter une application à partir de la galerie](./media/active-directory-saas-directions-microsoft-tutorial/IC749322.png "Ajouter une application à partir de la galerie")
6. Dans la **zone de recherche**, tapez **Directions on Microsoft**.
   
   ![Galerie d’applications](./media/active-directory-saas-directions-microsoft-tutorial/IC786878.png "Galerie d’applications")
7. Dans le volet des résultats, sélectionnez **Directions on Microsoft**, puis cliquez sur **Terminer** pour ajouter l’application.
   
   ![Scénario](./media/active-directory-saas-directions-microsoft-tutorial/IC793922.png "Scénario")
   
## <a name="configure-single-sign-on"></a>Configurer l’authentification unique

Cette section explique comment permettre aux utilisateurs de s’authentifier sur Directions on Microsoft avec leur compte Azure AD en utilisant la fédération basée sur le protocole SAML.

**Pour configurer l’authentification unique, suivez les étapes ci-dessous :**

1. Sur la page d’intégration d’applications **Directions on Microsoft** du Portail Azure Classic, cliquez sur **Configurer l’authentification unique** pour ouvrir la boîte de dialogue **Configurer l’authentification unique**.
   
   ![Activer l’authentification unique](./media/active-directory-saas-directions-microsoft-tutorial/IC786879.png "Activer l’authentification unique")
2. Dans la page **Comment voulez-vous que les utilisateurs se connectent à Directions on Microsoft**, sélectionnez **Authentification unique avec Microsoft Azure AD**, puis cliquez sur **Suivant**.
   
   ![Authentification unique avec Microsoft Azure AD](./media/active-directory-saas-directions-microsoft-tutorial/IC786880.png "Authentification unique avec Microsoft Azure AD")
3. Dans la page **Configurer l’URL de l’application**, dans la zone de texte, tapez **https://www.directionsonmicrosoft.com/user/login**, puis cliquez sur **Suivant**.
   
   ![Configurer l’URL de l’application](./media/active-directory-saas-directions-microsoft-tutorial/IC786881.png "Configurer l’URL de l’application")
4. Dans la page **Configurer l’authentification unique sur Directions on Microsoft**, cliquez sur **Télécharger les métadonnées**, puis enregistrez le fichier de métadonnées en local sur votre ordinateur.
   
   ![Configurer l’authentification unique](./media/active-directory-saas-directions-microsoft-tutorial/IC786882.png "Configurer l’authentification unique")
5. Envoyez le fichier de métadonnées à l’équipe du support technique Directions on Microsoft (*service@DirectionsOnMicrosoft.com*). Pour permettre à l’équipe du support technique Directions on Microsoft de trouver votre adhésion au site fédéré, indiquez les informations de votre entreprise dans votre e-mail.
   
   >[!NOTE]
   >L’authentification unique pour Directions on Microsoft doit être activée par l’équipe du support technique Directions on Microsoft. Vous recevrez une notification une fois l’authentification unique activée. 
   > 
6. Dans le portail Azure Classic, sélectionnez la confirmation de la configuration de l’authentification unique, puis cliquez sur **Terminer** pour fermer la boîte de dialogue **Configurer l’authentification unique**.
   
  ![Configurer l’authentification unique](./media/active-directory-saas-directions-microsoft-tutorial/IC786883.png "Configurer l’authentification unique")
   
## <a name="configure-user-provisioning"></a>Configurer l'approvisionnement de l'utilisateur

Aucun élément d’action ne vous permet de configurer l’approvisionnement des utilisateurs dans Directions on Microsoft.  

Quand un utilisateur affecté tente de se connecter à Directions on Microsoft à l’aide du volet d’accès, Directions on Microsoft vérifie l’existence de l’utilisateur. Si aucun compte d’utilisateur n’est disponible, Directions on Microsoft le crée automatiquement.

## <a name="assign-users"></a>Affecter des utilisateurs
Pour tester votre configuration, vous devez autoriser les utilisateurs d’Azure AD concernés à accéder à votre application.

**Pour affecter des utilisateurs à Directions on Microsoft, suivez les étapes ci-dessous :**

1. Dans le portail Azure Classic, créez un compte de test.
2. Sur la page d’intégration d’applications **Directions on Microsoft**, cliquez sur **Affecter des utilisateurs**.
   
   ![Affecter des utilisateurs](./media/active-directory-saas-directions-microsoft-tutorial/IC786884.png "Affecter des utilisateurs")
3. Sélectionnez votre utilisateur de test, cliquez sur **Affecter**, puis sur **Oui** pour confirmer votre affectation.
   
   ![Oui](./media/active-directory-saas-directions-microsoft-tutorial/IC767830.png "Oui")


