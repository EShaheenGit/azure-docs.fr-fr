---
title: "Utiliser les serveurs NPS existants pour exploiter les fonctionnalités d’Azure MFA | Microsoft Docs"
description: "L’extension de serveur NPS (Network Policy Server) pour Azure Multi-Factor Authentication est une solution simple permettant d’ajouter des fonctionnalités de vérification en deux étapes basées sur le cloud à votre infrastructure d’authentification existante."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: 
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/23/2017
ms.author: kgremban
ms.custom: H1Hack27Feb2017
ms.translationtype: Human Translation
ms.sourcegitcommit: db034a8151495fbb431f3f6969c08cb3677daa3e
ms.openlocfilehash: 58b289530e16c2a2e9bbe59b372c858ff22ad5ac
ms.contentlocale: fr-fr
ms.lasthandoff: 04/29/2017

---
# <a name="integrate-your-existing-nps-infrastructure-with-azure-multi-factor-authentication---public-preview"></a>Intégrer votre infrastructure NPS existante à Azure Multi-Factor Authentication - Version préliminaire publique

L’extension de serveur NPS (Network Policy Server) pour Azure MFA permet d’ajouter des fonctionnalités de MFA basées sur le cloud à votre infrastructure d’authentification à l’aide de vos serveurs existants. Avec l’extension NPS, vous pouvez ajouter des vérifications basées sur des appels téléphoniques, des SMS ou des applications mobiles à votre flux d’authentification existant sans avoir à installer, configurer et gérer les nouveaux serveurs. 

Cette extension a été créée pour les organisations qui souhaitent protéger des connexions VPN sans déployer le serveur Azure MFA. L’extension NPS joue le rôle d’adaptateur entre RADIUS et Azure MFA sur le cloud pour fournir un second facteur d’authentification pour les utilisateurs fédérés ou synchronisés. 

Lorsque vous utilisez l’extension NPS pour Azure MFA, le flux d’authentification inclut les composants suivants : 

1. **Le serveur NAS/VPN** reçoit les demandes des clients VPN et les convertit en demandes RADIUS à des serveurs NPS. 
2. **Le serveur NPS** se connecte à Active Directory pour procéder à l’authentification principale pour les demandes RADIUS et, en cas de réussite, les transmet à toutes les extensions installées.  
3. **L’extension NPS** déclenche une demande destinée à Azure MFA pour l’authentification secondaire. Une fois que l’extension reçoit la réponse, et si la demande MFA réussit, elle termine la demande d’authentification en fournissant au serveur NPS des jetons de sécurité qui incluent une revendication MFA, émise par Azure STS.  
4. **Azure MFA** communique avec Azure Active Directory pour récupérer les informations de l’utilisateur et procède à l’authentification secondaire à l’aide d’une méthode de vérification configurée par l’utilisateur.

Le diagramme suivant illustre ce flux de demande d’authentification de niveau supérieur : 

![Diagramme du flux d’authentification](./media/multi-factor-authentication-nps-extension/auth-flow.png)

## <a name="plan-your-deployment"></a>Planifier votre déploiement

L’extension NPS gère automatiquement la redondance, vous n’avez pas besoin d’une configuration spéciale. 

Vous pouvez créer autant de serveurs NPS compatibles avec Azure Multi-Factor Authentication que vous le souhaitez. Si vous installez plusieurs serveurs, vous devez utiliser un certificat client différent pour chacun d'entre eux. La création d’un certificat pour chaque serveur signifie que vous pouvez mettre à jour chaque certificat individuellement et que vous n’avez pas à craindre une interruption de service sur tous vos serveurs. 

Comme les serveurs VPN acheminent les demandes d’authentification, ils doivent connaître les nouveaux serveurs NPS compatibles avec Azure MFA. 

## <a name="prerequisites"></a>Composants requis

L’extension NPS est conçue pour fonctionner avec votre infrastructure existante. Vérifiez que les conditions préalables suivantes sont remplies avant de commencer.

### <a name="licenses"></a>Licences

L’extension NPS pour Azure MFA est disponible pour les clients disposant de [licences pour Azure MFA](multi-factor-authentication.md) (incluses dans Azure AD Premium, EMS ou un abonnement MFA).

### <a name="software"></a>Logiciel

Windows Server 2008 R2 SP1 ou version ultérieure.

### <a name="libraries"></a>Bibliothèques

Ces bibliothèques sont installées automatiquement avec l’extension. 
-    [Visual C++ Redistributable Packages pour Visual Studio 2013 (X64)](https://www.microsoft.com/download/details.aspx?id=40784)
-    [Module Microsoft Azure Active Directory pour Windows PowerShell version 1.1.166.0](https://connect.microsoft.com/site1164/Downloads/DownloadDetails.aspx?DownloadID=59185)

### <a name="azure-active-directory"></a>Azure Active Directory

Tous les utilisateurs de l’extension NPS doivent être synchronisés avec Azure Active Directory à l’aide d’Azure AD Connect et doivent être activés pour l’authentification MFA.

Lorsque vous installez l’extension, vous devez disposer de l’ID de répertoire et des informations d’identification de l’administrateur pour votre client Azure AD. Vous trouverez votre ID de répertoire dans le [portail Azure](https://portal.azure.com). Connectez-vous en tant qu’administrateur, sélectionnez l’icône **Azure Active Directory** sur la gauche, puis cliquez sur **Propriétés**. Copiez le GUID dans la zone **ID répertoire** et enregistrez. Vous utiliserez ce GUID comme ID client lors de l’installation de l’extension NPS.

![Identification de votre ID de répertoire dans les propriétés Azure Active Directory](./media/multi-factor-authentication-nps-extension/find-directory-id.png)

## <a name="prepare-your-environment"></a>Préparation de votre environnement

Avant d’installer l’extension NPS, vous souhaitez préparer votre environnement pour qu’il gère le trafic d’authentification. 

### <a name="enable-the-nps-role-on-a-domain-joined-server"></a>Activer le rôle NPS sur un serveur joint à un domaine

Le serveur NPS se connecte à Azure Active Directory et authentifie les demandes MFA. Choisissez un serveur pour ce rôle. Nous vous recommandons de choisir un serveur qui ne gère pas les demandes provenant d’autres services, car l’extension NPS génère des erreurs pour toutes les demandes qui ne sont pas RADIUS.

1. Sur votre serveur, ouvrez l’**Assistant d’ajout de rôles et fonctionnalités** à partir du menu Démarrage rapide du Gestionnaire de serveur.
2. Choisissez **Installation basée sur un rôle ou une fonctionnalité** pour votre type d’installation.
3. Sélectionnez le rôle de serveur **Services de stratégie et d’accès réseau**. Une fenêtre peut s’afficher pour vous informer des fonctionnalités requises pour exécuter ce rôle.
4. Suivez les instructions de l’Assistant jusqu'à la page de confirmation. Sélectionnez **Installer**. 

Maintenant que vous avez un serveur désigné pour NPS, vous devez également configurer ce serveur pour traiter les demandes RADIUS entrantes à partir de la solution VPN. 

### <a name="configure-your-vpn-solution-to-communicate-with-the-nps-server"></a>Configurer votre solution VPN pour communiquer avec le serveur NPS

En fonction de la solution VPN utilisée, la procédure servant à configurer votre stratégie d’authentification RADIUS n’est pas la même. Configurez cette stratégie pour qu’elle pointe vers votre serveur NPS RADIUS. 

### <a name="sync-domain-users-to-the-cloud"></a>Synchronisation des utilisateurs de domaine dans le cloud

Même si cette étape peut déjà avoir été effectuée sur votre client, il est judicieux de vérifier qu’Azure AD Connect a récemment synchronisé vos bases de données. 

1. Connectez-vous au [portail Azure](https://portal.azure.com) en tant qu’administrateur.
2. Sélectionnez **Azure Active Directory** > **Connexion Azure AD**
3. Vérifiez que votre état de synchronisation est **Activée** et que la dernière synchronisation a eu lieu il y a moins d’une heure.

Si vous avez besoin de lancer un nouveau cycle de synchronisations, suivez les instructions figurant dans [Planificateur Azure AD Connect Sync](../active-directory/connect/active-directory-aadconnectsync-feature-scheduler.md#start-the-scheduler).

### <a name="enable-users-for-mfa"></a>Autoriser des utilisateurs à utiliser MFA

Avant de déployer l’extension NPS complète, vous devez activer l’authentification MFA pour les utilisateurs pour lesquels vous souhaitez effectuer la vérification en deux étapes. De manière plus immédiate, pour tester l’extension lorsque vous la déployez, il vous faut au moins un compte de test entièrement inscrit pour Multi-Factor Authentication. 

Pour créer un compte de test, suivez la procédure suivante :
1. [Activez un compte pour MFA](multi-factor-authentication-get-started-user-states.md).
2. Accédez à un site web qui lance une authentification Azure AD, comme https://portal.azure.com.
3. [Inscrivez-vous à la vérification en deux étapes](./end-user/multi-factor-authentication-end-user-first-time.md).

Vos utilisateurs doivent également suivre ces étapes pour s’inscrire avant de pouvoir s’authentifier avec l’extension NPS.

## <a name="install-the-nps-extension"></a>Installer l’extension NPS

> [!IMPORTANT]
> Installez l’extension NPS sur un serveur autre que le point d’accès VPN. 

### <a name="download-and-install-the-nps-extension-for-azure-mfa"></a>Télécharger et installer l’extension NPS pour Azure MFA 

1.    [Téléchargez l’extension NPS](https://aka.ms/npsmfa) à partir du Centre de téléchargement Microsoft.
2.    Copiez le fichier binaire sur le serveur NPS (Network Policy Server) que vous souhaitez configurer.
3.    Exécutez le fichier *setup.exe* et suivez les instructions d’installation. Si vous rencontrez des erreurs, vérifiez que les deux bibliothèques de la section Configuration requise ont été installées avec succès.

### <a name="run-the-powershell-script"></a>Exécution du script PowerShell

Le programme d’installation crée un script PowerShell à cet emplacement : `C:\Program Files\Microsoft\AzureMfa\Config` (C:\ étant le lecteur d’installation). Ce script PowerShell effectue les opérations suivantes :

-    Vous pouvez créer un certificat auto-signé.
-    Associer la clé publique du certificat au principal du service sur Azure AD.
-    Stocker le certificat dans le magasin de certificats de l’ordinateur local.
-    Accorder l’accès à la clé privée du certificat à l’utilisateur réseau.
-    Redémarrer le serveur NPS.

À moins que vous ne souhaitiez utiliser vos propres certificats (au lieu des certificats auto-signés générés par le script PowerShell), exécutez le script PowerShell pour terminer l’installation. Si vous installez l’extension sur plusieurs serveurs, chacun d’eux doit avoir son propre certificat.

1. Exécutez Windows PowerShell en tant qu’administrateur.
2. Remplacez les répertoires.

   `cd "C:\Program Files\Microsoft\AzureMfa\Config"`

3. Exécutez le script PowerShell créé par le programme d’installation.

   `.\AzureMfaNpsExtnConfigSetup.ps1`

4. PowerShell vous invite à entrer votre ID client. Utilisez l’ID de répertoire GUID que vous avez copié à partir du portail Azure dans la section Configuration requise. 
5. Connectez-vous à Azure AD en tant qu’administrateur.
6. PowerShell affiche un message de réussite une fois le script terminé.  

Répétez ces étapes sur les serveurs NPS supplémentaires que vous souhaitez configurer pour l’équilibrage de charge. 

## <a name="configure-your-nps-extension"></a>Configurer votre extension NPS

Cette section comprend des considérations relatives à la conception ainsi que des suggestions visant à garantir la réussite des déploiements d’extension NPS.

### <a name="configurations-limitations"></a>Limitations des configurations

- L’extension NPS pour Azure MFA n’inclut pas les outils permettant de migrer les utilisateurs et les paramètres du serveur MFA vers le cloud. De ce fait, nous vous conseillons d’utiliser l’extension pour les nouveaux déploiements, plutôt que pour les déploiements existants. Si vous utilisez l’extension sur un déploiement existant, vos utilisateurs devront suivre de nouveau la procédure de vérification pour remplir leurs détails MFA dans le cloud.  
- L’extension NPS utilise l’UPN de l’annuaire Active Directory local pour identifier l’utilisateur sur Azure MFA afin de procéder à l’authentification secondaire. L’extension ne peut pas être configurée pour utiliser un identificateur autre que l’UPN (un ID de connexion alternatif ou un champ AD personnalisé, par exemple).  

### <a name="control-radius-clients-that-require-mfa"></a>Contrôler les clients RADIUS nécessitant l’authentification MFA

Une fois que vous activez l’authentification MFA pour un client RADIUS à l’aide de l’extension NPS, toutes les authentifications de ce client doivent utiliser la méthode MFA. Si vous souhaitez activer l’authentification MFA pour certains clients RADIUS et d’autres non, vous pouvez configurer deux serveurs NPS et installer l’extension sur un seul d’entre eux. Configurez les clients RADIUS pour lesquels vous souhaitez exiger l’authentification MFA de sorte qu’ils envoient des demandes au serveur NPS configuré avec l’extension, et les autres clients RADIUS pour qu’ils les envoient au serveur NPS non configuré avec l’extension.

### <a name="prepare-for-users-that-arent-enrolled-for-mfa"></a>Anticiper la présence d’utilisateurs qui ne sont pas inscrits pour l’authentification MFA

Si vous avez des utilisateurs qui ne sont pas inscrits pour l’authentification MFA, vous pouvez déterminer ce qui se passe lorsqu’ils tentent de s’authentifier. Utilisez le paramètre de Registre *REQUIRE_USER_MATCH* dans le chemin d’accès au Registre *HKLM\Software\Microsoft\AzureMFA* pour contrôler le comportement de la fonctionnalité. Ce paramètre ne dispose que d’une seule option de configuration :

| Clé | Valeur | Default |
| --- | ----- | ------- |
| REQUIRE_USER_MATCH | TRUE/FALSE | Non défini (équivaut à TRUE) |

L’objectif de ce paramètre est de déterminer l’action à exécuter lorsqu’un utilisateur n’est pas inscrit pour l’authentification MFA. Lorsque la clé n’existe pas, n’est pas définie ou est définie sur TRUE, et si l’utilisateur n’est pas inscrit, l’extension échoue à la demande d’authentification MFA. Lorsque la clé est définie sur FALSE et que l’utilisateur n’est pas inscrit, l’authentification s’effectue sans procéder à l’authentification MFA.

Vous pouvez choisir de créer cette clé et de lui affecter la valeur FALSE pour vos utilisateurs qui sont en cours d’intégration et ne sont peut-être pas encore inscrits pour l’authentification Azure MFA. Toutefois, étant donné que la définition de la clé permet aux utilisateurs qui ne sont pas inscrits pour l’authentification MFA de se connecter, vous devez supprimer cette clé avant de passer en production.

## <a name="troubleshooting"></a>Résolution des problèmes

### <a name="how-do-i-verify-that-the-client-cert-is-installed-as-expected"></a>Comment vérifier que le certificat client est installé comme prévu ?

Recherchez le certificat auto-signé créé par le programme d’installation dans le magasin de certificats et vérifiez que la clé privée dispose d’autorisations accordées à l’utilisateur **NETWORK SERVICE**. Le certificat porte un nom d’objet de type **CN \<tenantid\>, UO = Extension Microsoft NPS**

-------------------------------------------------------------

### <a name="how-can-i-verify-that-my-client-cert-is-associated-to-my-tenant-in-azure-active-directory"></a>Comment vérifier que mon certificat client est associé à mon client dans Azure Active Directory ?

Ouvrez une invite de commandes PowerShell et exécutez les commandes suivantes :

```
import-module MSOnline
Connect-MsolService
Get-MsolServicePrincipalCredential -AppPrincipalId "981f26a1-7f43-403b-a875-f8b09b8cd720" -ReturnKeyValues 1 
```

Ces commandes impriment tous les certificats qui associent votre client à votre instance de l’extension NPS dans votre session PowerShell. Recherchez votre certificat en exportant votre certificat client en tant que fichier « Codé à base 64 X.509 (.cer) » sans la clé privée et comparez-le à la liste de PowerShell.

Les horodatages Valid-From (Valide à partir de) et Valid-Until (Valide jusqu’à), qui sont dans un format explicite, peuvent être utilisés pour filtrer les certificats de toute évidence non appropriés si la commande en renvoie plusieurs.

-------------------------------------------------------------

### <a name="why-are-my-requests-failing-with-adal-token-error"></a>Pourquoi mes demandes échouent-elles avec une erreur de jeton ADAL ?

Cette erreur peut être due à plusieurs raisons. Suivez ces étapes pour résoudre le problème :

1. Redémarrez votre serveur NPS.
2. Vérifiez que le certificat client est installé comme prévu.
3. Vérifiez que le certificat est associé à votre client sur Azure AD.
4. Vérifiez que https://login.windows.net/ est accessible depuis le serveur exécutant l’extension.

-------------------------------------------------------------

### <a name="why-does-authentication-fail-with-an-error-in-http-logs-stating-that-the-user-is-not-found"></a>Pourquoi l’authentification échoue-t-elle avec une erreur dans les journaux HTTP indiquant que l’utilisateur est introuvable ?

Vérifiez qu’AD Connect est en cours d’exécution et que l’utilisateur se trouve dans Windows Active Directory et Azure Active Directory.

------------------------------------------------------------

### <a name="why-do-i-see-http-connect-errors-in-logs-with-all-my-authentications-failing"></a>Pourquoi existe-t-il des erreurs de connexion HTTP dans les journaux pour tous mes échecs d’authentification ?

Vérifiez que https://adnotifications.windowsazure.com est accessible à partir du serveur exécutant l’extension NPS.

## <a name="next-steps"></a>Étapes suivantes

Découvrez comment intégrer Azure MFA avec [Active Directory](multi-factor-authentication-get-started-server-dirint.md), [l’authentification RADIUS](multi-factor-authentication-get-started-server-radius.md) et [l’authentification LDAP](multi-factor-authentication-get-started-server-ldap.md).

