---
title: "Exemple de script Azure CLI - Créer une application ASP.NET Core dans un conteneur Docker | Microsoft Docs"
description: "Exemple de script Azure CLI - Créer une application ASP.NET Core dans un conteneur Docker"
services: appservice
documentationcenter: appservice
author: syntaxc4
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 3a2d1983-ff7b-476a-ac44-49ec2aabb31a
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
ms.openlocfilehash: 1876052306c5452dd0d183ce290ab6ff49e8fbae
ms.contentlocale: fr-fr
ms.lasthandoff: 05/15/2017

---

# <a name="create-an-aspnet-core-web-app-in-a-docker-container"></a>Créer une application web ASP.NET Core dans un conteneur Docker

Dans ce scénario, vous allez apprendre à créer un groupe de ressources, un plan App Service Linux et une application web, et à déployer une application ASP.NET Core à l’aide d’un conteneur Docker.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Exemple de script

[!code-azurecli-interactive[principal](../../../cli_scripts/app-service/deploy-linux-docker/deploy-linux-docker.sh?highlight=6 "Linux Docker")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Explication du script

Ce script utilise les commandes suivantes pour créer un groupe de ressources, une application web et toutes les ressources associées. Chaque commande du tableau renvoie à une documentation spécifique.

| Commande | Remarques |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#create) | Crée un groupe de ressources dans lequel toutes les ressources sont stockées. |
| [az appservice plan create](https://docs.microsoft.com/cli/azure/appservice/plan#create) | Crée un plan App Service. Cela équivaut à une batterie de serveurs pour votre application web Azure. |
| [az appservice web create](https://docs.microsoft.com/cli/azure/appservice/web#create) | Crée une application web Azure dans le plan App Service. |
| [az appservice web config container update](https://docs.microsoft.com/cli/azure/appservice/web/config/container#update) | Définit le conteneur Docker pour l’application web Azure. |

## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations sur l’interface Azure CLI, consultez la [documentation relative à l’interface Azure CLI](https://docs.microsoft.com/cli/azure/overview).

Vous trouverez des exemples supplémentaires de scripts CLI App Service dans la [documentation relative à Azure App Service](../app-service-cli-samples.md).

