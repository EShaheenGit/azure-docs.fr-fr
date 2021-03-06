---
title: "Équilibrer la charge des conteneurs dans un cluster DC/OS Azure | Microsoft Docs"
description: "Équilibrer la charge de plusieurs conteneurs dans un cluster DC/OS Azure Container Service."
services: container-service
documentationcenter: 
author: rgardler
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: Conteneurs, micro-services, DC/OS, Azure
ms.assetid: f0ab5645-2636-42de-b23b-4c3a7e3aa8bb
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/11/2016
ms.author: rogardle
translationtype: Human Translation
ms.sourcegitcommit: 6ea03adaabc1cd9e62aa91d4237481d8330704a1
ms.openlocfilehash: f8a001350c9e1ac50641c3ee4430849023233c60
ms.lasthandoff: 04/06/2017


---
# <a name="load-balance-containers-in-an-azure-container-service-dcos-cluster"></a>Équilibrer la charge des conteneurs dans un cluster DC/OS Azure Container Service
Dans cet article, nous allons découvrir comment créer un équilibreur de charge interne dans un cluster Azure Container Service géré par DC/OS à l’aide de Marathon-LB. Cela vous permettra de faire évoluer vos applications horizontalement. Vous pourrez également tirer parti des clusters d’agents publics et privés en plaçant vos équilibrages de charge sur le cluster public et vos conteneurs d’applications sur le cluster privé.

## <a name="prerequisites"></a>Composants requis
[Déployez une instance d’Azure Container Service](container-service-deployment.md) avec un orchestrator de type DC/OS, et [vérifiez que votre client peut se connecter à votre cluster](container-service-connect.md). 

## <a name="load-balancing"></a>Équilibrage de la charge
Il existe deux couches d’équilibrage de charge dans le cluster Container Service que nous allons créer : 

1. Azure Load Balancer fournit des points d’entrée publics (ceux auxquels les utilisateurs finaux ont recours). Il est automatiquement fourni par Azure Container Service. Par défaut, il est configuré pour ouvrir les ports 80, 443 et 8080.
2. L’outil Marathon Load Balancer (marathon-lb) achemine les demandes entrantes vers les instances de conteneur qui traitent ces demandes. L’outil marathon-lb s’adapte dynamiquement au fur et à mesure que les conteneurs fournissant notre service web sont mis à l’échelle. Par défaut, cet équilibreur de charge n’est pas fourni dans votre service Container Service, mais il est très facile à installer.

## <a name="marathon-load-balancer"></a>Équilibreur de charge Marathon
L’équilibreur de charge Marathon se reconfigure dynamiquement en fonction des conteneurs que vous avez déployés. Il est également résistant à la perte d’un conteneur ou d’un agent. Le cas échéant, Apache Mesos redémarre simplement le conteneur ailleurs, et l’outil marathon-lb s’adapte.

Pour installer l’outil Marathon Load Balancer, vous pouvez utiliser l’IU du site web DC/OS ou la ligne de commande DC/OS.

### <a name="install-marathon-lb-using-dcos-web-ui"></a>Installer l’outil Marathon-LB à l’aide de l’IU du site web DC/OS
1. Cliquez sur Universe (Univers).
2. Recherchez Marathon-LB.
3. Cliquez sur Install (Installer).

![Installation de l’outil marathon-lb via l’interface web DC/OS](./media/dcos/marathon-lb-install.png)

### <a name="install-marathon-lb-using-the-dcos-cli"></a>Installer l’outil Marathon-LB à l’aide de l’interface CLI DC/OS
Après avoir installé l’interface CLI DC/OS et vous être assuré que vous pouvez vous connecter à votre cluster, exécutez la commande suivante à partir de votre ordinateur client :

```bash
dcos package install marathon-lb
```

Cette commande installe automatiquement l’équilibreur de charge sur le cluster d’agents publics.

## <a name="deploy-a-load-balanced-web-application"></a>Déployer une application web à charge équilibrée
Maintenant que nous avons le package marathon-lb, nous pouvons déployer le conteneur d’applications dont nous souhaitons équilibrer la charge. Pour cet exemple, nous allons déployer un serveur web simple à l’aide de la configuration suivante :

```json
{
  "id": "web",
  "container": {
    "type": "DOCKER",
    "docker": {
      "image": "yeasy/simple-web",
      "network": "BRIDGE",
      "portMappings": [
        { "hostPort": 0, "containerPort": 80, "servicePort": 10000 }
      ],
      "forcePullImage":true
    }
  },
  "instances": 3,
  "cpus": 0.1,
  "mem": 65,
  "healthChecks": [{
      "protocol": "HTTP",
      "path": "/",
      "portIndex": 0,
      "timeoutSeconds": 10,
      "gracePeriodSeconds": 10,
      "intervalSeconds": 2,
      "maxConsecutiveFailures": 10
  }],
  "labels":{
    "HAPROXY_GROUP":"external",
    "HAPROXY_0_VHOST":"YOUR FQDN",
    "HAPROXY_0_MODE":"http"
  }
}

```

* Définissez la valeur `HAPROXY_0_VHOST` sur le nom de domaine complet de l’équilibreur de charge de vos agents. Il respecte la forme `<acsName>agents.<region>.cloudapp.azure.com`. Par exemple, si vous créez un cluster Container Service avec le nom `myacs` dans la région `West US`, le nom de domaine complet sera `myacsagents.westus.cloudapp.azure.com`. Vous pouvez également le trouver en recherchant l’équilibreur de charge dont le nom contient le terme « agent » lorsque vous parcourez les ressources du groupe de ressources que vous avez créé pour votre service Container Service dans le [portail Azure](https://portal.azure.com).
* Définissez le `servicePort` sur un port >= 10 000. Cela identifie le service en cours d’exécution dans ce conteneur ; marathon-lb s’en sert pour identifier les services sur lesquels il doit équilibrer les charges.
* Définissez l’étiquette `HAPROXY_GROUP` sur external (externe).
* Définissez `hostPort` sur 0. Cela signifie que Marathon allouera un port disponible de manière aléatoire.
* Définissez `instances` sur le nombre d’instances à créer. Vous pourrez toujours augmenter ou réduire ce nombre ultérieurement.

Il est intéressant de noter que par défaut, Marathon se déploie sur le cluster privé. Cela signifie que le déploiement ci-dessus sera accessible uniquement via votre équilibreur de charge, ce qui est généralement le comportement souhaité.

### <a name="deploy-using-the-dcos-web-ui"></a>Déployer à l’aide de l’IU du site web DC/OS
1. Visitez la page de Marathon à l’adresse http://localhost/marathon (après avoir configuré votre [tunnel SSH](container-service-connect.md)) et cliquez sur `Create Application`
2. Dans la boîte de dialogue `New Application`, cliquez sur `JSON Mode` dans le coin supérieur droit
3. Collez le code JSON ci-dessus dans l’éditeur.
4. Cliquez sur `Create Application`

### <a name="deploy-using-the-dcos-cli"></a>Déployer à l’aide de l’interface CLI DC/OS
Pour déployer cette application avec l’interface CLI DC/OS, il vous suffit de copier le code JSON ci-dessus dans un fichier appelé `hello-web.json`, puis exécutez :

```bash
dcos marathon app add hello-web.json
```

## <a name="azure-load-balancer"></a>Équilibrage de charge Azure
Par défaut, Azure Load Balancer expose les ports 80, 8080 et 443. Si vous utilisez l’un de ces trois ports (comme nous le faisons dans l’exemple ci-dessus), vous n’avez rien à faire. Vous devez pouvoir atteindre le nom de domaine complet de l’équilibreur de charge de votre agent. À chaque actualisation, vous atteindrez l’un de vos trois serveurs web par tourniquet (round robin). Toutefois, si vous utilisez un port différent, vous devez ajouter une règle de type tourniquet (round-robin) ainsi qu’une sonde sur l’équilibreur de charge pour le port que vous avez utilisé. Vous pouvez le faire depuis [l’interface de ligne de commande (CLI) Azure](../xplat-cli-azure-resource-manager.md), avec les commandes `azure network lb rule create` et `azure network lb probe create`. Vous pouvez également procéder à l’aide du portail Azure.

## <a name="additional-scenarios"></a>Autres cas de figure
Un scénario possible serait l’utilisation de différents domaines pour exposer des services différents. Par exemple : 

mydomain1.com -> Azure LB:80 -> marathon-lb:10001 -> mycontainer1:33292  
mydomain2.com -> Azure LB:80 -> marathon-lb:10002 -> mycontainer2:22321

Pour ce faire, consultez la section consacrée aux [hôtes virtuels](https://mesosphere.com/blog/2015/12/04/dcos-marathon-lb/), qui explique comment associer des domaines à des chemins d’accès marathon-lb spécifiques.

Vous pouvez également exposer des ports différents et les remapper vers le service correct derrière marathon-lb. Par exemple : 

Azure lb:80 -> marathon-lb:10001 -> mycontainer:233423  
Azure lb:8080 -> marathon-lb:1002 -> mycontainer2:33432

## <a name="next-steps"></a>Étapes suivantes
Pour en savoir plus sur [marathon-lb](https://dcos.io/docs/1.7/usage/service-discovery/marathon-lb/), consultez la documentation DC/OS.


