---
title: "Récupération d’urgence de compte d’intégration Logic Apps B2B Integration - Azure App Service | Microsoft Docs"
description: "Récupération d’urgence Logic Apps B2B"
services: logic-apps
documentationcenter: .net,nodejs,java
author: padmavc
manager: anneta
editor: 
ms.assetid: cf44af18-1fe5-41d5-9e06-cc57a968207c
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/10/2017
ms.author: padmavc
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 197df490690754730425231f358fde31d17dcfad
ms.contentlocale: fr-fr
ms.lasthandoff: 05/10/2017


---

# <a name="logic-apps-b2b-cross-region-disaster-recovery"></a>Récupération d’urgence inter-régions Logic Apps B2B
Les charges de travail B2B impliquent des transactions monétaires telles que des commandes et des factures. Pour les entreprises, une récupération rapide est essentielle pour respecter les SLA de niveau entreprise convenus avec leurs partenaires lors d’un sinistre. Cet article montre comment créer un plan de continuité des activités pour les charges de travail B2B. 

* Préparation à la récupération d’urgence 
* Basculer vers la région secondaire lors d’un sinistre 
* Revenir à la région primaire après sinistre

## <a name="disaster-recovery-readiness"></a>Préparation à la récupération d’urgence  

1. Identifier une région secondaire et créer un [compte d’intégration](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) dans cette région 

2. Ajoutez des partenaires, des schémas et des contrats pour les flux de messages requis où l’état d’exécution doit être répliqué sur le compte d’intégration de la région secondaire.

    > [!Tip]
    > Vérifiez la cohérence entre les régions dans la convention d’affectation de noms pour les artefacts de compte d’intégration. 
    > 
    > 

3. Pour extraire l’état d’exécution de la région primaire, créez une application logique dans la région secondaire.  Il doit avoir un **déclencheur** et une **action**.  Le déclencheur doit se connecter au compte d’intégration de la région primaire tandis que l’action doit se connecter au compte d’intégration de la région secondaire.  Selon l’intervalle de temps, le déclencheur interroge la table d’état d’exécution de la région primaire, extrait les nouveaux enregistrements le cas échéant et met à jour leurs actions dans le compte d’intégration de la région secondaire. Cela permet d’obtenir un état de runtime incrémentiel depuis la région primaire vers la région secondaire.

4. La continuité des activités dans le compte d’intégration Logic Apps est conçue pour prendre en charge des protocoles basés sur B2B : X12, AS2 et EDIFACT.  Pour une procédure détaillée, sélectionnez les liens correspondants.

5. Il est recommandé de déployer toutes les ressources de la région primaire dans une région secondaire. Les ressources de la région primaire incluent Azure SQL Database ou Azure Cosmos DB, Azure Service Bus/Azure Event Hubs (utilisés pour la messagerie), la gestion des API Azure et la fonctionnalité Logic Apps d’Azure App Service.   

6. Établissez une connexion entre la région primaire et la région secondaire. Pour extraire l’état d’exécution d’une région primaire, créez une application logique dans une région secondaire. Il doit avoir un déclencheur et une action. Le déclencheur doit se connecter au compte d’intégration d’une région primaire. Le déclencheur doit se connecter au compte d’intégration d’une région secondaire. Selon l’intervalle de temps, le déclencheur interroge la table d’état d’exécution de la région primaire et extrait les nouveaux enregistrements, le cas échéant. L’action les met à jour dans le compte d’intégration de la région secondaire. Ce processus permet d’obtenir un état de runtime incrémentiel depuis la région primaire vers la région secondaire.

La continuité des activités dans le compte d’intégration Logic Apps prend en charge les protocoles B2B X12, AS2 et EDIFACT. Pour obtenir des instructions détaillées sur l’utilisation de X12 et AS2, consultez les sections de cet articles consacrées à [X12](../logic-apps/logic-apps-enterprise-integration-b2b-business-continuity.md#x12) et [AS2](../logic-apps/logic-apps-enterprise-integration-b2b-business-continuity.md#as2).

## <a name="fail-over-to-a-secondary-region-during-a-disaster-event"></a>Basculer vers une région secondaire lors d’un sinistre
Lors d’un sinistre, lorsque la région primaire n’est pas disponible pour la continuité des activités, dirigez le trafic vers la région secondaire. Une région secondaire permet à une entreprise de récupérer rapidement ses fonctions de manière à respecter ses objectifs de point de récupération et de temps de récupération convenus avec ses partenaires. Elle contribue en outre à réduire les efforts nécessaires pour basculer d’une région à l’autre. 

Il existe une latence attendue lorsque des numéros de contrôle sont copiés depuis une région primaire vers une région secondaire. Pour ne pas envoyer de numéros de contrôle générés en double aux partenaires lors d’un sinistre, il est recommandé d’augmenter les numéros de contrôle dans les accords de région secondaire au moyen [d’applets de commande PowerShell](https://blogs.msdn.microsoft.com/david_burgs_blog/2017/03/09/fresh-of-the-press-new-azure-powershell-cmdlets-for-upcoming-x12-connector-disaster-recovery).

## <a name="fall-back-to-a-primary-region-post-disaster-event"></a>Revenir à une région primaire après un sinistre
Pour revenir à une région primaire lorsqu’elle est de nouveau disponible, procédez comme suit :

1. Cessez d’accepter les messages des partenaires dans la région secondaire.  

2. Augmentez les numéros de contrôle générés pour tous les accords de région primaire au moyen [d’applets de commande PowerShell](https://blogs.msdn.microsoft.com/david_burgs_blog/2017/03/09/fresh-of-the-press-new-azure-powershell-cmdlets-for-upcoming-x12-connector-disaster-recovery).  

3. Dirigez le trafic de la région secondaire vers la région primaire.

4. Vérifiez que l’application logique créée dans la région secondaire pour extraire l’état d’exécution de la région primaire est activée.

## <a name="x12"></a>X 12 
La continuité des activités pour les documents EDI X12 documents repose sur les numéros de contrôle :
* Numéros de contrôle reçus (messages entrants) de la part des partenaires  
* Numéros de contrôle générés (messages sortants) et envoyés aux partenaires 
    
    > [!Tip]
    > Vous pouvez également utiliser le [modèle de démarrage rapide X12](https://azure.microsoft.com/documentation/templates/201-logic-app-x12-disaster-recovery-replication/) pour créer des applications logiques. La création d’un compte d’intégration primaire et d’un compte d’intégration secondaire est nécessaire pour utiliser le modèle. Le modèle permet de créer deux applications logiques, une pour les numéros de contrôle reçus et l’autre pour les numéros de contrôle générés. Les déclencheurs et actions respectifs sont créés dans les applications logiques, ce qui permet de connecter le déclencheur connecté au compte d’intégration primaire et de connecter l’action au compte d’intégration secondaire.
    > 
    >

### <a name="control-numbers-received-from-partners"></a>Numéros de contrôle reçus de la part des partenaires

1. Activer les contrôles en double dans les paramètres de réception de l’accord   
![Recherche X12](./media/logic-apps-enterprise-integration-b2b-business-continuity/dupcheck.png)  

2. Créez une [application logique](../logic-apps/logic-apps-create-a-logic-app.md) dans la région secondaire. 

3. Recherchez **X12** et sélectionnez **X12 - lorsqu’un numéro de contrôle reçu est modifié**.   
![Recherche X12](./media/logic-apps-enterprise-integration-b2b-business-continuity/X12recevicedCN1.png)

4. Le déclencheur vous invite à établir une connexion à un compte d’intégration. Le déclencheur doit être connecté au compte d’intégration d’une région primaire. Entrez un nom de connexion, sélectionnez votre **compte d’intégration de la région primaire** dans la liste et cliquez sur **Créer**.  
![Nom du compte d’intégration de la région primaire](./media/logic-apps-enterprise-integration-b2b-business-continuity/X12recevicedCN2.png)

5. Le paramètre **DateTime pour démarrer la synchronisation des numéros de contrôle** est facultatif. La **Fréquence** peut être définie sur **Jour**, **Heure**, **Minute** ou **Seconde** avec un intervalle.  
![DateTime et fréquence](./media/logic-apps-enterprise-integration-b2b-business-continuity/X12recevicedCN3.png)

6. Sélectionnez **Nouvelle étape** > **Ajouter une action**.    
![Ajouter une action](./media/logic-apps-enterprise-integration-b2b-business-continuity/X12recevicedCN4.png)

7. Lancez une recherche sur **X12** et sélectionnez **X12 - Ajouter ou mettre à jour un numéro de contrôle reçu**.   
![Modification des numéros de contrôle reçus](./media/logic-apps-enterprise-integration-b2b-business-continuity/X12recevicedCN5.png)

8. Pour connecter une action à un compte d’intégration d’une région secondaire, sélectionnez **Modifier la connexion** > **Ajouter une nouvelle connexion** pour obtenir la liste des comptes d’intégration disponibles. Entrez un nom de connexion, sélectionnez votre **compte d’intégration de la région secondaire** dans la liste et cliquez sur **Créer**.   
![Nom du compte d’intégration de la région secondaire](./media/logic-apps-enterprise-integration-b2b-business-continuity/X12recevicedCN6.png)

9. Sélectionnez le contenu dynamique et enregistrez l’application logique. 
![Contenu dynamique](./media/logic-apps-enterprise-integration-b2b-business-continuity/X12recevicedCN7.png)

10. Selon l’intervalle de temps, le déclencheur interroge la table des numéros de contrôle de la région primaire et extrait les nouveaux enregistrements. L’action les met à jour dans le compte d’intégration de la région secondaire. S’il n’y a aucune mise à jour, le déclencheur affiche l’état **Ignoré**.
![Table des numéros de contrôle](./media/logic-apps-enterprise-integration-b2b-business-continuity/X12recevicedCN8.png)

### <a name="control-numbers-generated-and-sent-to-partners"></a>Numéros de contrôle générés et envoyés aux partenaires
1. Créez une [application logique](../logic-apps/logic-apps-create-a-logic-app.md) dans la région secondaire.

2. Lancez une recherche sur **X12** et sélectionnez **X12 - Lorsqu’un numéro de contrôle généré est modifié**.  
![Modification des numéros de contrôle générés](./media/logic-apps-enterprise-integration-b2b-business-continuity/X12generatedCN1.png)

3. Le déclencheur vous invite à établir une connexion à un compte d’intégration. Le déclencheur doit être connecté au compte d’intégration d’une région primaire. Entrez un nom de connexion, sélectionnez votre **compte d’intégration de la région primaire** dans la liste et cliquez sur **Créer**.   
![Nom du compte d’intégration de la région primaire](./media/logic-apps-enterprise-integration-b2b-business-continuity/X12generatedCN2.png) 

4. Le paramètre **DateTime pour démarrer la synchronisation des numéros de contrôle** est facultatif. La **Fréquence** peut être définie sur **Jour**, **Heure**, **Minute** ou **Seconde** avec un intervalle.  
![DateTime et fréquence](./media/logic-apps-enterprise-integration-b2b-business-continuity/X12generatedCN3.png)  

5. Sélectionnez **Nouvelle étape** > **Ajouter une action**.  
![Ajouter une action](./media/logic-apps-enterprise-integration-b2b-business-continuity/X12generatedCN4.png)

6. Lancez une recherche sur **X12** et sélectionnez **X12 - Ajouter ou mettre à jour un numéro de contrôle généré**.   
![Ajout ou mise à jour de numéros de contrôle générés](./media/logic-apps-enterprise-integration-b2b-business-continuity/X12generatedCN5.png)

7. Pour connecter une action à un compte d’intégration secondaire, sélectionnez **Modifier la connexion** > **Ajouter une nouvelle connexion** pour obtenir la liste des comptes d’intégration disponibles. Entrez un nom de connexion, sélectionnez votre **compte d’intégration de la région secondaire** dans la liste et cliquez sur **Créer**.   
![Nom du compte d’intégration de la région secondaire](./media/logic-apps-enterprise-integration-b2b-business-continuity/X12generatedCN6.png)

8. Sélectionnez le contenu dynamique et enregistrez l’application logique. 
![Contenu dynamique](./media/logic-apps-enterprise-integration-b2b-business-continuity/X12generatedCN7.png)

9. Selon l’intervalle de temps, le déclencheur interroge la table des numéros de contrôle de la région primaire et extrait les nouveaux enregistrements. L’action les met à jour dans le compte d’intégration de la région secondaire. S’il n’y a aucune mise à jour, le déclencheur affiche l’état **Ignoré**.  
![Table des numéros de contrôle](./media/logic-apps-enterprise-integration-b2b-business-continuity/X12generatedCN8.png)

En fonction de l’intervalle de temps, l’état d’exécution incrémentiel est dupliqué d’une région primaire à une région secondaire. Lors d’un sinistre, lorsque la région primaire n’est pas disponible, dirigez le trafic vers la région secondaire pour la continuité des activités. 

## <a name="as2"></a>AS2 
La continuité des activités pour les documents qui utilisent le protocole AS2 est basée sur l’ID de message et la valeur MIC.

> [!Tip]
    > Vous pouvez également utiliser le [modèle de démarrage rapide AS2](https://github.com/Azure/azure-quickstart-templates/pull/3302) pour créer des applications logiques. La création d’un compte d’intégration primaire et d’un compte d’intégration secondaire est nécessaire pour utiliser le modèle. Ce modèle permet de créer une application logique qui comporte un déclencheur et une action. L’application logique crée une connexion entre le déclencheur et un compte d’intégration primaire et entre l’action et un compte d’intégration secondaire.
    > 
    >

1. Créez une [application logique](../logic-apps/logic-apps-create-a-logic-app.md) dans la région secondaire.  

2. Recherchez **AS2** et sélectionnez **AS2 - Lorsqu’une valeur MIC est créée**.   
![Recherche AS2](./media/logic-apps-enterprise-integration-b2b-business-continuity/AS2messageid1.png)

3. Un déclencheur vous invite à établir une connexion à un compte d’intégration. Le déclencheur doit être connecté au compte d’intégration d’une région primaire. Entrez un nom de connexion, sélectionnez votre **compte d’intégration de la région primaire** dans la liste et cliquez sur **Créer**.   
![Nom du compte d’intégration de la région primaire](./media/logic-apps-enterprise-integration-b2b-business-continuity/AS2messageid2.png)

4. Le paramètre **DateTime de démarrage de la synchronisation des valeurs MIC** est facultatif. La **Fréquence** peut être définie sur **Jour**, **Heure**, **Minute** ou **Seconde** avec un intervalle.   
![DateTime et fréquence](./media/logic-apps-enterprise-integration-b2b-business-continuity/AS2messageid3.png)

5. Sélectionnez **Nouvelle étape** > **Ajouter une action**.  
![Ajouter une action](./media/logic-apps-enterprise-integration-b2b-business-continuity/AS2messageid4.png)

6. Recherchez **AS2** et sélectionnez **AS2 - Ajouter ou mettre à jour un MIC**.  
![Ajout ou mise à jour d’un MIC](./media/logic-apps-enterprise-integration-b2b-business-continuity/AS2messageid5.png)

7. Pour connecter une action à un compte d’intégration secondaire, sélectionnez **Modifier la connexion** > **Ajouter une nouvelle connexion** pour obtenir la liste des comptes d’intégration disponibles. Entrez un nom de connexion, sélectionnez votre **compte d’intégration de la région secondaire** dans la liste et cliquez sur **Créer**.    
![Nom du compte d’intégration de la région secondaire](./media/logic-apps-enterprise-integration-b2b-business-continuity/AS2messageid6.png)

8. Sélectionnez le contenu dynamique et enregistrez l’application logique.   
![Contenu dynamique](./media/logic-apps-enterprise-integration-b2b-business-continuity/AS2messageid7.png)

9. Selon l’intervalle de temps, le déclencheur interroge la table de la région primaire et extrait les nouveaux enregistrements. L’action les met à jour dans le compte d’intégration de la région secondaire. S’il n’y a aucune mise à jour, le déclencheur affiche l’état **Ignoré**.  
![Table de la région primaire](./media/logic-apps-enterprise-integration-b2b-business-continuity/AS2messageid8.png)

En fonction de l’intervalle de temps, l’état d’exécution incrémentiel est dupliqué de la région primaire à la région secondaire. Lors d’un sinistre, lorsque la région primaire n’est pas disponible, dirigez le trafic vers la région secondaire pour la continuité des activités. 

## <a name="next-steps"></a>Étapes suivantes
En savoir plus sur le [suivi des messages B2B](logic-apps-monitor-b2b-message.md).   


