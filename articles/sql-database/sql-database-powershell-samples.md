---
title: Exemples Azure PowerShell pour SQL Database | Microsoft Docs
description: "Exemples Azure PowerShell - Scripts vous permettant de créer et de gérer des serveurs, des pools élastiques, des bases de données et des pare-feu Azure SQL Database."
services: sql-database
documentationcenter: sql-database
author: CarlRabeler
manager: jhubbard
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: sql-database
ms.custom: sample
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: sql-database
ms.workload: database
ms.date: 03/07/2017
ms.author: janeng
ms.translationtype: Human Translation
ms.sourcegitcommit: 95b8c100246815f72570d898b4a5555e6196a1a0
ms.openlocfilehash: c9216c4b5309a91aff56d220307e2757b81012a9
ms.contentlocale: fr-fr
ms.lasthandoff: 05/18/2017

---

# <a name="azure-powershell-samples-for-azure-sql-database"></a>Exemples Azure PowerShell pour Azure SQL Database

Le tableau suivant comprend des liens vers des exemples de scripts Azure PowerShell pour Azure SQL Database.

| |  |
|---|---|
|**Créer une base de données unique et un pool élastique**||
| [Créer une base de données unique et configurer une règle de pare-feu](scripts/sql-database-create-and-configure-database-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Crée une base de données SQL Azure unique et configure une règle de pare-feu au niveau du serveur. |
| [Créer des pools élastiques et déplacer les bases de données regroupées](scripts/sql-database-move-database-between-pools-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Crée des pools élastiques, déplace des bases de données regroupées et modifie les niveaux de performances.|
|**Configurer la géoréplication et le basculement**||
| [Configurer et basculer une base de données unique à l’aide de la géoréplication active](scripts/sql-database-setup-geodr-and-failover-database-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Configure la géoréplication active pour une base de données Azure SQL unique et la fait basculer vers le réplica secondaire. |
| [Configurer et basculer une base de données regroupée à l’aide de la géoréplication active](scripts/sql-database-setup-geodr-and-failover-pool-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Configure la géoréplication active pour une base de données Azure SQL dans un pool élastique et la fait basculer vers le réplica secondaire. |
|**Mettre à l’échelle une base de données unique et un pool élastique**||
| [Mettre à l’échelle une base de données unique](scripts/sql-database-monitor-and-scale-database-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Surveille les mesures de performances d’une base de données Azure SQL, l’adapte à un niveau de performances supérieur et crée une règle d’alerte sur l’une des mesures de performances. |
| [Mettre à l’échelle un pool élastique](scripts/sql-database-monitor-and-scale-pool-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Surveille les mesures de performances d’un pool élastique, l’adapte à un niveau de performances supérieur et crée une règle d’alerte sur l’une des mesures de performances.  |
| **Audit et détection des menaces** |
| [Configurer l’audit et la détection des menaces](scripts/sql-database-auditing-and-threat-detection-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Configure les stratégies d’audit et de détection des menaces pour une base de données Azure SQL. |
| **Restaurer, copier et importer une base de données**||
| [Restaurer une base de données](scripts/sql-database-restore-database-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Restaure une base de données Azure SQL à partir d’une sauvegarde géo-redondante et restaure une base de données Azure SQL supprimée dans la dernière sauvegarde. |
| [Copier une base de données sur un nouveau serveur](scripts/sql-database-copy-database-to-new-server-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Crée une copie d’une base de données Azure SQL existante sur un nouveau serveur Azure SQL. |
| [Importer une base de données à partir d’un fichier bacpac](scripts/sql-database-import-from-bacpac-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Importe une base de données vers un serveur Azure SQL à partir d’un fichier bacpac. |
|||

