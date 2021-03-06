---
title: "Test des performances et de la mise à l’échelle Azure Cosmos DB | Microsoft Docs"
description: "Découvrez comment effectuer un test des performances et de la mise à l’échelle avec Azure Cosmos DB"
keywords: Tester les performances
services: cosmos-db
author: arramac
manager: jhubbard
editor: 
documentationcenter: 
ms.assetid: f4c96ebd-f53c-427d-a500-3f28fe7b11d0
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/24/2017
ms.author: arramac
ms.translationtype: Human Translation
ms.sourcegitcommit: a643f139be40b9b11f865d528622bafbe7dec939
ms.openlocfilehash: f85be2c605ec5dadd9296486d3a1357c39e40b64
ms.contentlocale: fr-fr
ms.lasthandoff: 05/31/2017


---
# <a name="performance-and-scale-testing-with-azure-cosmos-db"></a>Test des performances et de la mise à l’échelle avec Azure Cosmos DB
Le test des performances et de la mise à l’échelle est une étape essentielle du développement d’applications. Pour de nombreuses applications, la couche de base de données a un impact significatif sur les performances et l’évolutivité globales et est, par conséquent, un composant critique du test de performances. [Azure Cosmos DB](https://azure.microsoft.com/services/documentdb/) a été spécialement conçu pour une mise à l’échelle élastique et des performances prévisibles et, par conséquent, convient parfaitement aux applications nécessitant un niveau de base de données hautes performances. 

Cet article est une référence pour les développeurs implémentant des suites de tests de performances pour leurs charges de travail Cosmos DB ou évaluant Cosmos DB pour des scénarios d’applications hautes performances. Il se concentre principalement sur les tests de performances isolés de la base de données, mais inclut également les meilleures pratiques relatives aux applications de production.

Après avoir lu cet article, vous serez en mesure de répondre aux questions suivantes :   

* Où puis-je trouver un exemple d’application cliente .NET pour le test des performances de Cosmos DB ? 
* Comment atteindre des niveaux de débit élevés avec Cosmos DB à partir de mon application cliente ?

Pour commencer avec le code, téléchargez le projet à partir de l’[exemple de test des performances Azure Cosmos DB](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/documentdb-benchmark). 

> [!NOTE]
> L’objectif de cette application est d’illustrer les bonnes pratiques pour optimiser les performances à partir de Cosmos DB avec un petit nombre d’ordinateurs clients. Elle n’a pas pour but de démontrer la capacité maximale du service, qui peut s’ajuster sans limite.
> 
> 

Si vous recherchez des options de configuration côté client pour améliorer les performances de Cosmos DB, consultez la page [Azure Cosmos DB performance tips](performance-tips.md)(Conseils relatifs aux performances d’Azure Cosmos DB).

## <a name="run-the-performance-testing-application"></a>Exécution de l’application de test des performances
La manière la plus rapide de commencer consiste à compiler et exécuter l’exemple de code .NET ci-dessous, comme décrit dans la procédure ci-dessous. Vous pouvez également examiner le code source et implémenter des configurations similaires dans vos propres applications clientes.

**Étape 1 :** téléchargez le projet à partir de l’[exemple de test des performances Azure Cosmos DB](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/documentdb-benchmark) ou répliquez le référentiel GitHub.

**Étape 2 :** modifiez les paramètres pour EndpointUrl, AuthorizationKey, CollectionThroughput et DocumentTemplate (facultatif) dans App.config.

> [!NOTE]
> Avant l’approvisionnement des collections à débit élevé, reportez-vous à la [Page de tarification](https://azure.microsoft.com/pricing/details/documentdb/) pour estimer les coûts par collection. Cosmos DB facture indépendamment le stockage et le débit à l’heure pour vous permettre de réduire les coûts en supprimant ou en diminuant le débit de vos collections DocumentDB après le test.
> 
> 

**Étape 3 :** compilez et exécutez l’application console à partir de la ligne de commande. Un résultat similaire à ce qui suit s’affiche normalement :

    Summary:
    ---------------------------------------------------------------------
    Endpoint: https://docdb-scale-demo.documents.azure.com:443/
    Collection : db.testdata at 50000 request units per second
    Document Template*: Player.json
    Degree of parallelism*: 500
    ---------------------------------------------------------------------

    DocumentDBBenchmark starting...
    Creating database db
    Creating collection testdata
    Creating metric collection metrics
    Retrying after sleeping for 00:03:34.1720000
    Starting Inserts with 500 tasks
    Inserted 661 docs @ 656 writes/s, 6860 RU/s (18B max monthly 1KB reads)
    Inserted 6505 docs @ 2668 writes/s, 27962 RU/s (72B max monthly 1KB reads)
    Inserted 11756 docs @ 3240 writes/s, 33957 RU/s (88B max monthly 1KB reads)
    Inserted 17076 docs @ 3590 writes/s, 37627 RU/s (98B max monthly 1KB reads)
    Inserted 22106 docs @ 3748 writes/s, 39281 RU/s (102B max monthly 1KB reads)
    Inserted 28430 docs @ 3902 writes/s, 40897 RU/s (106B max monthly 1KB reads)
    Inserted 33492 docs @ 3928 writes/s, 41168 RU/s (107B max monthly 1KB reads)
    Inserted 38392 docs @ 3963 writes/s, 41528 RU/s (108B max monthly 1KB reads)
    Inserted 43371 docs @ 4012 writes/s, 42051 RU/s (109B max monthly 1KB reads)
    Inserted 48477 docs @ 4035 writes/s, 42282 RU/s (110B max monthly 1KB reads)
    Inserted 53845 docs @ 4088 writes/s, 42845 RU/s (111B max monthly 1KB reads)
    Inserted 59267 docs @ 4138 writes/s, 43364 RU/s (112B max monthly 1KB reads)
    Inserted 64703 docs @ 4197 writes/s, 43981 RU/s (114B max monthly 1KB reads)
    Inserted 70428 docs @ 4216 writes/s, 44181 RU/s (115B max monthly 1KB reads)
    Inserted 75868 docs @ 4247 writes/s, 44505 RU/s (115B max monthly 1KB reads)
    Inserted 81571 docs @ 4280 writes/s, 44852 RU/s (116B max monthly 1KB reads)
    Inserted 86271 docs @ 4273 writes/s, 44783 RU/s (116B max monthly 1KB reads)
    Inserted 91993 docs @ 4299 writes/s, 45056 RU/s (117B max monthly 1KB reads)
    Inserted 97469 docs @ 4292 writes/s, 44984 RU/s (117B max monthly 1KB reads)
    Inserted 99736 docs @ 4192 writes/s, 43930 RU/s (114B max monthly 1KB reads)
    Inserted 99997 docs @ 4013 writes/s, 42051 RU/s (109B max monthly 1KB reads)
    Inserted 100000 docs @ 3846 writes/s, 40304 RU/s (104B max monthly 1KB reads)

    Summary:
    ---------------------------------------------------------------------
    Inserted 100000 docs @ 3834 writes/s, 40180 RU/s (104B max monthly 1KB reads)
    ---------------------------------------------------------------------
    DocumentDBBenchmark completed successfully.


**Étape 4 (si nécessaire) :** le débit signalé (RU/s) à partir de l’outil doit être identique ou supérieur au débit approvisionné de la collection. Dans le cas contraire, le fait d’augmenter la valeur de DegreeOfParallelism par petits incréments peut vous aider à atteindre la limite. Si le débit de votre application cliente se stabilise, le lancement de plusieurs instances de l’application sur les mêmes ordinateurs ou sur d’autres machines vous permettra d’atteindre la limite approvisionnée sur les différentes instances. Si vous avez besoin d’aide pour cette étape, envoyez un e-mail à askdocdb@microsoft.com ou remplissez un ticket de support à partir du [portail Azure](https://portal.azure.com).

Une fois l’application en cours d’exécution, vous pouvez essayer différentes [stratégies d’indexation](indexing-policies.md) et [niveaux de cohérence](consistency-levels.md) pour comprendre leur impact sur le débit et la latence. Vous pouvez également examiner le code source et implémenter des configurations similaires dans vos propres suites de test ou applications de production.

## <a name="next-steps"></a>Étapes suivantes
Dans cet article, nous avons vu comment effectuer des tests de performances et d’évolutivité avec Cosmos DB à l’aide d’une application console .NET. Consultez les liens ci-dessous pour plus d’informations sur le fonctionnement de Cosmos DB.

* [Exemple de test des performances Azure Cosmos DB](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/documentdb-benchmark)
* [Options de configuration côté client permettant d’améliorer les performances d’Azure Cosmos DB](performance-tips.md)
* [Guide de partitionnement et de mise à l’échelle dans Azure Cosmos DB](partition-data.md)
* [Collection DocumentDB et niveaux de performance](performance-levels.md)
* [Documentation du Kit de développement logiciel (SDK) DocumentDB .NET sur MSDN](https://msdn.microsoft.com/library/azure/dn948556.aspx)
* [Exemples .NET DocumentDB](https://github.com/Azure/azure-documentdb-net)
* [Blog Azure Cosmos DB comportant des conseils relatifs aux performances](https://azure.microsoft.com/blog/2015/01/20/performance-tips-for-azure-documentdb-part-1-2/)


