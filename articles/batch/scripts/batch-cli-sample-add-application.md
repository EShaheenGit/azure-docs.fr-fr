---
title: "Exemple de script Azure CLI - Ajout d’une application dans Batch | Microsoft Docs"
description: "Exemple de script Azure CLI - Ajout d’une application dans Batch"
services: batch
documentationcenter: 
author: annatisch
manager: daryls
editor: tysonn
ms.assetid: 
ms.service: batch
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/02/2017
ms.author: antisch
ms.translationtype: Human Translation
ms.sourcegitcommit: 9568210d4df6cfcf5b89ba8154a11ad9322fa9cc
ms.openlocfilehash: 5d057eaf32867aedc95d58c5185e2be1f9385ec0
ms.contentlocale: fr-fr
ms.lasthandoff: 05/15/2017

---

# <a name="adding-applications-to-azure-batch-with-azure-cli"></a>Ajout d’applications dans Azure Batch avec Azure CLI

Ce script explique comment configurer une application à utiliser avec une tâche ou un pool Azure Batch. Pour configurer cette application, placez votre fichier exécutable et toutes ses éventuelles dépendances dans un fichier .zip. Dans cet exemple, le fichier .zip est appelé « my-application-exe.zip ».

## <a name="prerequisites"></a>Composants requis

- Installez Azure CLI en suivant les instructions fournies dans le [Guide d’installation d’Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli), si ce n’est déjà fait.
- Créez un compte Azure si vous n’en avez pas. Pour un exemple de script qui crée un compte, voir [Créer un compte Batch avec Azure CLI](https://docs.microsoft.com/azure/batch/scripts/batch-cli-sample-create-account).

## <a name="sample-script"></a>Exemple de script

[!code-azurecli[principal](../../../cli_scripts/batch/add-application/add-application.sh "Ajouter une application")]

## <a name="clean-up-application"></a>Supprimer l’application

Une fois l’exemple de script ci-dessus exécuté, lancez les commandes suivantes pour supprimer l’application et tous les packages associés qui ont été chargés.

```azurecli
az batch application package delete -g myresourcegroup -n mybatchaccount --application-id myapp --version 1.0 --yes
az batch application delete -g myresourcegroup -n mybatchaccount --application-id myapp --yes
```

## <a name="script-explanation"></a>Explication du script

Ce script utilise les commandes suivantes pour créer une application et charger un package d’application.
Chaque commande de la table renvoie à une documentation spécifique.

| Commande | Remarques |
|---|---|
| [az batch application create](https://docs.microsoft.com/cli/azure/batch/application#create) | Crée une application.  |
| [az batch application set](https://docs.microsoft.com/cli/azure/batch/application#set) | Met à jour les propriétés d’une application.  |
| [az batch application package create](https://docs.microsoft.com/cli/azure/batch/application/package#create) | Ajoute un package d’application à l’application spécifiée.  |

## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations sur l’interface Azure CLI, consultez la [documentation relative à l’interface Azure CLI](https://docs.microsoft.com/cli/azure/overview).

Vous trouverez des exemples supplémentaires de scripts CLI Batch dans la [documentation relative à la CLI Azure Batch](../batch-cli-samples.md).

