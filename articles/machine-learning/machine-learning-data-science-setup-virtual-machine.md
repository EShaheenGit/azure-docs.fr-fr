---
title: "Configurer une machine virtuelle comme serveur IPython Notebook pour des analyses avancées | Microsoft Docs"
description: "Configurez une machine virtuelle Azure pour l’utiliser dans un environnement de science des données avec un serveur IPython à des fins d’analyse avancée."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 818617c1-048e-49c2-b941-f9a983f93998
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: xibingao;bradsev
translationtype: Human Translation
ms.sourcegitcommit: 503f5151047870aaf87e9bb7ebf2c7e4afa27b83
ms.openlocfilehash: 5fb930cb71fe008ba63d2068bb36643f40259e76
ms.lasthandoff: 03/29/2017


---
# <a name="set-up-an-azure-virtual-machine-as-an-ipython-notebook-server-for-advanced-analytics"></a>Configurer une machine virtuelle Azure comme serveur IPython Notebook pour des analyses avancées
Cette rubrique explique comment approvisionner et configurer une machine virtuelle Azure pour l’analyse avancée utilisable au sein d’un environnement de science des données. La machine virtuelle Windows est configurée avec des outils connexes, tels que Notebook IPython, l’Explorateur de stockage Azure, AzCopy, ainsi que d’autres utilitaires utiles pour les projets d’analyse avancée. Par exemple, l’Explorateur de stockage Azure et AzCopy facilitent le chargement de données dans le stockage d’objets blob Azure depuis votre ordinateur local ou le téléchargement de ces données vers votre ordinateur local à partir du stockage d’objets blob.

## <a name="create-vm"></a>Étape 1 : créer une machine virtuelle Azure à usage général
Si vous disposez déjà d’une machine virtuelle Azure et que vous souhaitez simplement configurer un serveur Notebook IPython sur cette machine, vous pouvez ignorer cette étape et passer directement à l’ [Étape 2 : ajouter un point de terminaison pour Notebook IPython à une machine virtuelle existante](#add-endpoint).

Avant de démarrer la procédure de création d’une machine virtuelle sur Azure, vous devez déterminer la taille de la machine requise pour le traitement des données du projet concerné. Les machines de taille réduite comportent moins de mémoire et de cœurs de processeur que les machines de grande taille, mais se révèlent également moins coûteuses. Pour obtenir la liste des différents types de machine et des prix correspondants, consultez la page <a href="http://azure.microsoft.com/pricing/details/virtual-machines/" target="_blank">Tarification des machines virtuelles</a>.

1. Connectez-vous au <a href="https://manage.windowsazure.com" target="_blank">portail Azure Classic</a>, puis cliquez sur **Nouveau** dans le coin inférieur gauche. Une fenêtre s’affiche. Sélectionnez **CALCUL** -> **MACHINE VIRTUELLE** -> **DE LA GALERIE**.
   
    ![Create workspace][24]
2. Choisissez l’une des images suivantes :
   
   * Windows Server 2012 R2 Datacenter
   * Expérience Windows Server Essentials (Windows Server 2012 R2)
     
     Puis cliquez sur la flèche pointant vers la droite dans le coin inférieur droit pour accéder à la page de configuration suivante.
     
     ![Create workspace][25]
3. Entrez un nom pour la machine virtuelle à créer, sélectionnez la taille et la puissance de la machine (A3, par défaut) en fonction des données que cette dernière devra traiter (taille de la mémoire et nombre de cœurs de processeur), puis entrez un nom d’utilisateur et un mot de passe pour la machine. Ensuite, cliquez sur la flèche pointant vers la droite pour accéder à la page de configuration suivante.
   
    ![Create workspace][26]
4. Sélectionnez le nom de **RÉGION/GROUPE D’AFFINITÉS/RÉSEAU VIRTUEL** qui contient le **COMPTE DE STOCKAGE** que vous prévoyez d’utiliser pour cette machine virtuelle, puis sélectionnez ce compte de stockage. Ajoutez un point de terminaison au bas du champ **POINTS DE TERMINAISON** en entrant son nom (« IPython » dans le cas présent). Vous pouvez indiquer la chaîne de votre choix pour le **NOM** du point de terminaison, et tout entier compris entre 0 et 65536 **disponible** en guise de **PORT PUBLIC**. Le **PORT PRIVÉ** doit être défini sur **9999**. Vous devez **éviter** d’utiliser des ports publics déjà attribués pour des services Internet. L’article concernant les <a href="http://www.chebucto.ns.ca/~rakerman/port-table.html" target="_blank">ports associés aux services Internet</a> répertorie les ports qui sont attribués et qui doivent donc être évités.
   
    ![Create workspace][27]
5. Cliquez sur la coche pour démarrer le processus d’approvisionnement de la machine virtuelle.
   
    ![Create workspace][28]

L’exécution de ce processus peut nécessiter entre 15 et 25 minutes. Une fois créée, la machine virtuelle doit présenter l’état **En cours d’exécution**.

![Create workspace][29]

## <a name="add-endpoint"></a>Étape 2 : ajouter un point de terminaison pour Notebook IPython à une machine virtuelle existante
Si vous avez créé la machine virtuelle en suivant les instructions de l’étape 1, le point de terminaison pour Notebook IPython a déjà été ajouté, et vous pouvez donc ignorer cette étape.

Si la machine virtuelle existe déjà et que vous devez ajouter un point de terminaison pour Notebook IPython que vous allez installer à l’étape 3 ci-après, commencez par vous connecter au portail Azure Classic, sélectionnez la machine virtuelle, puis ajoutez le point de terminaison pour le serveur Notebook IPython. La figure ci-dessous contient une capture d’écran du portail après l’ajout du point de terminaison pour Notebook IPython sur une machine virtuelle Windows.

![Create workspace][17]

## <a name="run-commands"></a>Étape 3 : installer Notebook IPython et les autres outils connexes
Une fois la machine virtuelle créée, utilisez le protocole RDP (Remote Desktop Protocol) pour vous connecter à la machine virtuelle Windows. Pour plus d’informations, consultez [Connexion à une machine virtuelle exécutant Windows Server](../virtual-machines/windows/classic/connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json). Ouvrez **l’invite de commandes** (et **non la fenêtre Commande Powershell**) en tant **qu’administrateur**, puis exécutez la commande suivante.

    set script='https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/MachineSetup/Azure_VM_Setup_Windows.ps1'

    @powershell -NoProfile -ExecutionPolicy unrestricted -Command "iex ((new-object net.webclient).DownloadString(%script%))"

Une fois l’installation terminée, le serveur Notebook IPython est automatiquement lancé dans le répertoire *C:\\Users\\\<nom_utilisateur\>\\Documents\\IPython Notebooks*.

Lorsque vous y êtes invité, entrez un mot de passe pour Notebook IPython et le mot de passe de l’administrateur de la machine. Ceci permet à Notebook IPython de s’exécuter en tant que service sur la machine.

## <a name="access"></a>Étape 4 : accéder à Notebook IPython à partir d’un navigateur web
Pour accéder au serveur Notebook IPython, ouvrez un navigateur web, puis entrez la chaîne *https://&#60;nom DNS de la machine virtuelle>:&#60;numéro du port public>* dans la zone de texte de l’URL. Dans cette chaîne, la variable *&#60;numéro du port public>* doit correspondre au numéro de port que vous avez spécifié durant l’ajout du point de terminaison de Notebook IPython.

La variable *&#60;nom DNS de la machine virtuelle>* correspond au nom DNS de la machine virtuelle, qui est accessible par le biais du portail Azure Classic. Après vous être connecté au portail Azure Classic, cliquez sur **MACHINES VIRTUELLES**, sélectionnez la machine que vous avez créée, puis sélectionnez **TABLEAU DE BORD**. Le nom DNS s’affiche sous la forme suivante :

![Create workspace][19]

Vous verrez apparaître un message d’avertissement signalant *qu’il existe un problème avec le certificat de sécurité de ce site web* (Internet Explorer) ou que *votre connexion n’est pas privée* (Chrome), comme illustré dans les figures ci-après. Cliquez sur **Poursuivre avec ce site web (non recommandé)** (Internet Explorer) ou sur **Paramètres avancés**, puis sur **Continuer vers le site &#60;*Nom DNS*> (dangereux)** (Chrome) pour continuer. Puis entrez le mot de passe que vous avez indiqué précédemment pour accéder à Notebook IPython.

**Internet Explorer :**
![Créer un espace de travail][20]

**Chrome :**
![Créer un espace de travail][21]

Une fois que vous êtes connecté à Notebook IPython, le répertoire *DataScienceSamples* s’affiche sur le navigateur. Ce répertoire contient des exemples de notebooks IPython qui sont partagés par Microsoft pour permettre aux utilisateurs d’exécuter des tâches liées à la science des données. Ces exemples de notebooks IPython sont récupérés sur les machines virtuelles à partir du [**référentiel GitHub**](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/iPythonNotebooks) durant le processus de configuration du serveur Notebook IPython. Microsoft gère ce dépôt et le met régulièrement à jour. Les utilisateurs peuvent se connecter au dépôt GitHub pour obtenir les derniers exemples de notebooks IPython.
![Create workspace][18]

## <a name="upload"></a>Étape 5 : charger un notebook IPython figurant sur un ordinateur local vers le serveur Notebook IPython
Les utilisateurs peuvent aisément charger un notebook IPython de leur ordinateur local vers le serveur Notebook IPython exécuté sur les machines virtuelles. Une fois que vous vous êtes connecté au serveur Notebook IPython dans un navigateur web, cliquez sur le **répertoire** dans lequel le notebook IPython sera chargé. Ensuite, dans l’ **Explorateur de fichiers**de l’ordinateur local, sélectionnez le fichier .ipynb Notebook IPython à charger, puis faites-le glisser vers le répertoire Notebook IPython dans le navigateur web. Cliquez sur le bouton **Télécharger** pour charger le fichier .ipynb vers le serveur Notebook IPython. D’autres utilisateurs peuvent alors commencer à utiliser ce fichier dans leur navigateur web.

![Create workspace][22]

![Create workspace][23]

## <a name="shutdown"></a>Arrêter et libérer une machine virtuelle inutilisée
Le service Azure Virtual Machines est facturé au tarif du **paiement à l’utilisation**. Pour vous assurer que vous n’êtes pas facturé lorsque vous n’utilisez pas votre machine virtuelle, cette dernière doit être définie sur l’état **Arrêté (désalloué)** quand elle est inutilisée.

> [!NOTE]
> Si vous arrêtez la machine virtuelle depuis cette dernière (à l’aide des options d’alimentation Windows), la machine virtuelle est arrêtée, mais reste allouée. Pour être certain de ne plus être facturé, arrêtez toujours les machines virtuelles à partir du [portail Azure Classic](http://manage.windowsazure.com/). Vous pouvez également arrêter la machine virtuelle par le biais de Powershell en appelant **ShutdownRoleOperation** avec « PostShutdownAction » défini sur « StoppedDeallocated ».
> 
> 

Pour arrêter et libérer la machine virtuelle :

1. Connectez-vous au [portail Azure Classic](http://manage.windowsazure.com/) avec votre compte.  
2. Sélectionnez **MACHINES VIRTUELLES** dans la barre de navigation gauche.
3. Dans la liste des machines virtuelles, cliquez sur le nom de votre machine virtuelle, puis accédez à la page **TABLEAU DE BORD** .
4. Au bas de la page, cliquez sur **ARRÊT**.

![Arrêt de la machine virtuelle][15]

Cette opération libère la machine virtuelle, mais ne la supprime pas. Vous pouvez redémarrer votre machine virtuelle à tout moment à partir du portail Azure Classic.

## <a name="your-azure-vm-is-ready-to-use-whats-next"></a>Étapes suivantes une fois votre machine virtuelle Azure prête à être utilisée
Votre machine virtuelle est désormais prête à l’emploi dans vos exercices de science des données. Cette machine est également utilisable sous la forme d’un serveur Notebook IPython pour l’exploration et le traitement des données, ainsi que pour l’exécution d’autres tâches avec Azure Machine Learning et le processus TDSP (Team Data Science Process).

Les étapes suivantes du processus TDSP (Team Data Science Process) sont présentées dans le [Parcours d’apprentissage](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) et peuvent inclure des étapes de déplacement, de traitement et d’échantillonnage des données dans HDInsight en vue d’en extraire de l’information pertinente avec Azure Machine Learning.

[15]: ./media/machine-learning-data-science-setup-virtual-machine/vmshutdown.png
[17]: ./media/machine-learning-data-science-setup-virtual-machine/add-endpoints-after-creation.png
[18]: ./media/machine-learning-data-science-setup-virtual-machine/sample-ipnbs.png
[19]: ./media/machine-learning-data-science-setup-virtual-machine/dns-name-and-host-name.png
[20]: ./media/machine-learning-data-science-setup-virtual-machine/browser-warning-ie.png
[21]: ./media/machine-learning-data-science-setup-virtual-machine/browser-warning.png
[22]: ./media/machine-learning-data-science-setup-virtual-machine/upload-ipnb-1.png
[23]: ./media/machine-learning-data-science-setup-virtual-machine/upload-ipnb-2.png
[24]: ./media/machine-learning-data-science-setup-virtual-machine/create-virtual-machine-1.png
[25]: ./media/machine-learning-data-science-setup-virtual-machine/create-virtual-machine-2.png
[26]: ./media/machine-learning-data-science-setup-virtual-machine/create-virtual-machine-3.png
[27]: ./media/machine-learning-data-science-setup-virtual-machine/create-virtual-machine-4.png
[28]: ./media/machine-learning-data-science-setup-virtual-machine/create-virtual-machine-5.png
[29]: ./media/machine-learning-data-science-setup-virtual-machine/create-virtual-machine-6.png

