---
title: "Guide pratique pour vider et restaurer une base de données Azure pour PostgreSQL | Microsoft Docs"
description: "Décrit comment extraire une base de données PostgreSQL dans un fichier de vidage et restaurer la base de données PostgreSQL à partir d’un fichier d’archive créé par la commande pg_dump dans la base de données Azure pour PostgreSQL."
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonh
ms.assetid: 
ms.service: postgresql-database
ms.tgt_pltfrm: portal
ms.topic: article
ms.date: 05/10/2017
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: b056b2aee8b3d82077f61c18e3a660f3562ae068
ms.contentlocale: fr-fr
ms.lasthandoff: 05/10/2017

---
# <a name="migrate-your-postgresql-database-using-dump-and-restore"></a>Migration de votre base de données PostgreSQL par vidage et restauration
Vous pouvez utiliser la commande [pg_dump](https://www.postgresql.org/docs/9.3/static/app-pgdump.html) pour extraire une base de données PostgreSQL vers un fichier de vidage, et la commande [pg_restore](https://www.postgresql.org/docs/9.3/static/app-pgrestore.html) pour restaurer la base de données PostgreSQL à partir d’un fichier d’archive créé par pg_dump.

## <a name="prerequisites"></a>Composants requis
Pour parcourir ce guide pratique, vous avez besoin des éléments suivants :
- Un [serveur Azure Database pour PostgreSQL](quickstart-create-server-database-portal.md) avec des règles de pare-feu autorisant l’accès et la base de données sous-jacente.
- Utilitaires de ligne de commande [pg_dump](https://www.postgresql.org/docs/9.6/static/app-pgdump.html) et [pg_restore](https://www.postgresql.org/docs/9.6/static/app-pgrestore.html) installés

Suivez les étapes ci-dessous pour vider et restaurer votre base de données PostgreSQL.

## <a name="create-a-dump-file-using-pgdump-that-contains-the-data-to-be-loaded"></a>Création d’un fichier de vidage à l’aide de pg_dump qui contient les données à charger
Pour sauvegarder une base de données PostgreSQL existante sur site ou dans une machine virtuelle, exécutez la commande suivante :
```bash
pg_dump -Fc -v --host=<host> --username=<name> --dbname=<database name> > <database>.dump
```
Par exemple, si vous avez un serveur local contenant une base de données appelée **testdb**
```bash
pg_dump -Fc -v --host=localhost --username=masterlogin --dbname=testdb > testdb.dump
```

## <a name="restore-the-data-into-the-target-azure-database-for-postrgesql-using-pgrestore"></a>Restauration des données dans la base de données cible pour PostrgeSQL à l’aide de pg_restore
Une fois que vous avez créé la base de données cible, vous pouvez utiliser la commande pg_restore et le paramètre -d, --dbname pour restaurer les données dans la base de données cible à partir du fichier de vidage.
```bash
pg_restore -v –-host=<server name> --port=<port> --username=<user@servername> --dbname=<target database name> <database>.dump
```
Dans cet exemple, nous allons restaurer les données dans la base de données **mypgsqldb** sur le serveur cible **mypgserver-20170401.postgres.database.azure.com** à partir du fichier de vidage généré **testdb.dump**
```bash
pg_restore -v --host=mypgserver-20170401.postgres.database.azure.com --port=5432 --username=mylogin@mypgserver-20170401 --dbname=mypgsqldb testdb.dump
```

## <a name="next-steps"></a>Étapes suivantes
- Pour migrer une base de données PostgreSQL par exportation et importation, consultez la rubrique [Migration de votre base de données PostgreSQL par exportation et importation](howto-migrate-using-export-and-import.md)
