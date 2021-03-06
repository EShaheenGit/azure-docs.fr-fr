---
title: "Déployer un conteneur Linux Docker ASP.NET Core sur un hôte Docker distant | Microsoft Docs"
description: "Découvrez comment utiliser Visual Studio Tools pour Docker pour déployer une application web ASP.NET Core dans un conteneur Docker fonctionnant sur une machine virtuelle hôte Azure Docker"
services: azure-container-service
documentationcenter: .net
author: mlearned
manager: douge
editor: 
ms.assetid: e5e81c5e-dd18-4d5a-a24d-a932036e78b9
ms.service: azure-container-service
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/08/2016
ms.author: mlearned
translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: 7169b6f2d9738abd9651120be96bb1cf209ea85d
ms.lasthandoff: 04/03/2017


---
# <a name="deploy-an-aspnet-container-to-a-remote-docker-host"></a>Déployer un conteneur ASP.NET sur un hôte Docker distant
## <a name="overview"></a>Vue d'ensemble
Docker est un moteur de conteneur léger, semblable à certains égards à une machine virtuelle, que vous pouvez utiliser pour héberger des applications et des services.
Ce didacticiel vous guide dans l’extension [Visual Studio 2015 Tools pour Docker](http://aka.ms/DockerToolsForVS) pour déployer une application ASP.NET Core sur un hôte Docker sur Azure à l’aide de PowerShell.

## <a name="prerequisites"></a>Composants requis
Les éléments suivants sont nécessaires pour suivre ce didacticiel :

* Créer une machine virtuelle hôte Azure Docker, comme le décrit la rubrique [Utiliser Docker Machine avec Azure](virtual-machines/linux/docker-machine.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* Installez [Visual Studio 2015 Update 3](https://go.microsoft.com/fwlink/?LinkId=691129)
* [Kit de développement logiciel (SDK) Microsoft ASP.NET Core 1.0](https://go.microsoft.com/fwlink/?LinkID=809122)
* Installez [Visual Studio 2015 Tools pour Docker - Version préliminaire](http://aka.ms/DockerToolsForVS)

## <a name="1-create-an-aspnet-core-web-app"></a>1. Créez une application web ASP.NET Core
La procédure suivante vous guidera dans la création d’une application ASP.NET 5 Core qui sera utilisée dans ce didacticiel.

[!INCLUDE [create-aspnet5-app](../includes/create-aspnet5-app.md)]

## <a name="2-add-docker-support"></a>2. Ajouter la prise en charge Docker
[!INCLUDE [create-aspnet5-app](../includes/vs-azure-tools-docker-add-docker-support.md)]

## <a name="3-use-the-dockertaskps1-powershell-script"></a>3. Utilisez le Script PowerShell DockerTask.ps1
1. Ouvrez une invite de commandes PowerShell vers le répertoire racine de votre projet. 
   
   ```
   PS C:\Src\WebApplication1>
   ```
2. Confirmez l’exécution de l’hôte distant. L’état (State) doit indiquer « Running » (En cours d’exécution) 
   
   ```
   docker-machine ls
   NAME         ACTIVE   DRIVER   STATE     URL                        SWARM   DOCKER    ERRORS
   MyDockerHost -        azure    Running   tcp://xxx.xxx.xxx.xxx:2376         v1.10.3
   ```
   
   > [!NOTE]
   > Si vous utilisez la version bêta de Docker, votre hôte ne sera pas répertorié ici.
   > 
   > 
3. Générez l’application à l’aide du paramètre -Build
   
   ```
   PS C:\Src\WebApplication1> .\Docker\DockerTask.ps1 -Build -Environment Release -Machine mydockerhost
   ```  
   
   > [!NOTE]
   > Si vous utilisez la version bêta de Docker, omettez l’argument -Machine
   > 
   > ```
   > PS C:\Src\WebApplication1> .\Docker\DockerTask.ps1 -Build -Environment Release 
   > ```  
   > 
   > 
4. Exécutez l’application à l’aide du paramètre -Run
   
   ```
   PS C:\Src\WebApplication1> .\Docker\DockerTask.ps1 -Run -Environment Release -Machine mydockerhost
   ```
   
   > [!NOTE]
   > Si vous utilisez la version bêta de Docker, omettez l’argument -Machine
   > 
   > ```
   > PS C:\Src\WebApplication1> .\Docker\DockerTask.ps1 -Run -Environment Release 
   > ```
   > 
   > 
   
   Une fois Docker terminé, vous devriez obtenir un résultat semblable à ce qui suit :
   
   ![Affichez votre application][3]

[0]:./media/vs-azure-tools-docker-hosting-web-apps-in-docker/docker-props-in-solution-explorer.png
[1]:./media/vs-azure-tools-docker-hosting-web-apps-in-docker/change-docker-machine-name.png
[2]:./media/vs-azure-tools-docker-hosting-web-apps-in-docker/launch-application.png
[3]:./media/vs-azure-tools-docker-hosting-web-apps-in-docker/view-application.png

