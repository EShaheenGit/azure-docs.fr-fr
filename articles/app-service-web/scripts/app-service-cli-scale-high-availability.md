---
title: "Exemple de script Azure CLI - Mettre à l’échelle une application web dans le monde entier avec une architecture haute disponibilité | Microsoft Docs"
description: "Exemple de script Azure CLI - Mettre à l’échelle une application web dans le monde entier avec une architecture haute disponibilité"
services: appservice
documentationcenter: appservice
author: syntaxc4
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: e4033a50-0e05-4505-8ce8-c876204b2acc
ms.service: app-service
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 03/20/2017
ms.author: cfowler
ms.custom: mvc
ms.translationtype: Human Translation
ms.sourcegitcommit: 9568210d4df6cfcf5b89ba8154a11ad9322fa9cc
ms.openlocfilehash: 5c467f35351a9e0c6261967b6ed6e6b58cfab682
ms.contentlocale: fr-fr
ms.lasthandoff: 05/15/2017

---

# <a name="scale-a-web-app-worldwide-with-a-high-availability-architecture"></a>Mettre à l’échelle une application web dans le monde entier avec une architecture haute disponibilité

Dans ce scénario, vous créez un groupe de ressources, deux plans App Service, deux applications web, un profil Traffic Manager et deux points de terminaison Traffic Manager. Une fois cette procédure terminée, vous bénéficiez d’une architecture haute disponibilité qui offre une disponibilité globale de votre application web en fonction de la plus faible latence réseau.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Exemple de script

[!code-azurecli-interactive[principal](../../../cli_scripts/app-service/scale-geographic/scale-geographic.sh "Échelle géographique")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Explication du script

Ce script utilise les commandes suivantes pour créer un groupe de ressources, une application web, un profil Traffic Manager et toutes les ressources associées. Chaque commande du tableau renvoie à une documentation spécifique.

| Commande | Remarques |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#create) | Crée un groupe de ressources dans lequel toutes les ressources sont stockées. |
| [az appservice plan create](https://docs.microsoft.com/cli/azure/appservice/plan#create) | Crée un plan App Service. Cela équivaut à une batterie de serveurs pour votre application web Azure. |
| [az appservice web create](https://docs.microsoft.com/cli/azure/appservice/web#create) | Crée une application web Azure dans le plan App Service. |
| [az network traffic-manager profile create](https://docs.microsoft.com/cli/azure/network/traffic-manager/profile#create) | Crée un profil Azure Traffic Manager. |
| [az network traffic-manager endpoint create](https://docs.microsoft.com/cli/azure/network/traffic-manager/endpoint#create) | Ajoute un point de terminaison à un profil Azure Traffic Manager. |

## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations sur l’interface Azure CLI, consultez la [documentation relative à l’interface Azure CLI](https://docs.microsoft.com/cli/azure/overview).

Vous trouverez des exemples supplémentaires de scripts CLI App Service dans la [documentation relative à Azure App Service](../app-service-cli-samples.md).

