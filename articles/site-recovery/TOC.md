# Vue d'ensemble
## [Qu’est-ce que Site Recovery ?](site-recovery-overview.md)
## Comment Site Recovery fonctionne-t-il ?
### [Architecture Azure vers Azure](site-recovery-azure-to-azure-architecture.md)
### [Architecture VMware vers Azure](site-recovery-architecture-vmware-to-azure.md)
### [Architecture Hyper-V vers Azure](site-recovery-architecture-hyper-v-to-azure.md)
### [Réplication vers une architecture de site secondaire](site-recovery-architecture-to-secondary-site.md)
## [Quelles charges de travail pouvez-vous protéger ?](site-recovery-workload.md)
## Matrice de prise en charge Site Recovery
### [Prise en charge d’Azure vers Azure](site-recovery-support-matrix-azure-to-azure.md)
### [Prise en charge local vers Azure](site-recovery-support-matrix-to-azure.md)
### [Prise en charge local vers site secondaire](site-recovery-support-matrix-to-sec-site.md)
## [FORUM AUX QUESTIONS](site-recovery-faq.md)
## [Voir une présentation](https://azure.microsoft.com/resources/videos/index/?services=site-recovery)

# Prise en main
## [Réplication de machines virtuelles Azure (aperçu)](site-recovery-azure-to-azure.md)
## [Répliquer des serveurs physiques dans Azure](site-recovery-physical-servers-to-azure.md)
## [Répliquer des machines virtuelles Hyper-V dans Azure (avec VMM)](site-recovery-vmm-to-azure.md)
## [Réplication de machines virtuelles Hyper-V dans Azure](site-recovery-hyper-v-site-to-azure.md)
## [Répliquer des machines virtuelles Hyper-V vers un site secondaire (avec VMM)](site-recovery-vmm-to-vmm.md)
## [Répliquer des machines virtuelles VMware et des serveurs physiques vers un site secondaire](site-recovery-vmware-to-vmware.md)
## [Répliquer des machines virtuelles VMware dans Azure dans le cadre d’un déploiement mutualisé (CSP)](site-recovery-multi-tenant-support-vmware-using-csp.md)

# Procédure
## Planification
### [Conditions préalables pour la réplication Azure](site-recovery-azure-to-azure-prereq.md)
### Planifier la mise en réseau
#### [Planifier la mise en réseau pour la réplication d’Azure vers Azure (aperçu)](site-recovery-azure-to-azure-networking-guidance.md)
#### [Planifier la mise en réseau pour la réplication de l’ordinateur local](site-recovery-network-design.md)
#### [Planifier le mappage réseau pour la réplication de machines virtuelles Azure](site-recovery-network-mapping-azure-to-azure.md)
#### [Planifier le mappage réseau pour la réplication de machines virtuelles Hyper-V](site-recovery-network-mapping.md)
### Planifier la capacité et l’extensibilité
#### [Planifier la capacité pour la réplication de VMware vers Azure](site-recovery-plan-capacity-vmware.md)
#### [Deployment Planner pour la réplication de VMware vers Azure](site-recovery-deployment-planner.md)
#### [Capacity Planner pour la réplication Hyper-V](site-recovery-capacity-planner.md)
### [Planifier l’accès en fonction du rôle pour la réplication de machines virtuelles](site-recovery-role-based-linked-access-control.md)
## Déployer
### [Réplication de machines virtuelles VMware dans Azure](vmware-walkthrough-overview.md)
#### [Étape 1 : examen de l’architecture](vmware-walkthrough-architecture.md)
#### [Étape 2 :vérifier les conditions préalables et les limitations](vmware-walkthrough-prerequisites.md)
#### [Étape 3 : planifier la capacité](vmware-walkthrough-capacity.md)
#### [Étape 4 : planifier la mise en réseau](vmware-walkthrough-network.md)
#### [Étape 5 : préparer Azure](vmware-walkthrough-prepare-azure.md)
#### [Étape 6 : préparer VMware](vmware-walkthrough-prepare-vmware.md)
#### [Étape 7 : créer un coffre](vmware-walkthrough-create-vault.md)
#### [Étape 8 : configurer la source et la cible](vmware-walkthrough-source-target.md)
#### [Étape 9 : configurer une stratégie de réplication](vmware-walkthrough-replication.md)
#### [Étape 10 : installer le service Mobilité](vmware-walkthrough-install-mobility.md)
#### [Étape 11 : activer la réplication](vmware-walkthrough-enable-replication.md)
#### [Étape 12 : exécution d’un test de basculement](vmware-walkthrough-test-failover.md)
## Configuration
### Configurer l’environnement source
#### [Environnement source pour VMware vers Azure](site-recovery-set-up-vmware-to-azure.md)
#### [Environnement source pour serveur physique vers Azure](site-recovery-set-up-physical-to-azure.md)
### Configurer l’environnement cible
#### [Environnement cible pour VMware vers Azure](site-recovery-prepare-target-vmware-to-azure.md)
#### [Environnement cible pour serveur physique vers Azure](site-recovery-prepare-target-physical-to-azure.md)
### [Configurer les paramètres de réplication](site-recovery-setup-replication-settings-vmware.md)
### [Déployer le service Mobilité pour la réplication VMware](site-recovery-vmware-to-azure-install-mob-svc.md)
#### [Déployer le service Mobilité avec System Center Configuration Manager](site-recovery-install-mobility-service-using-sccm.md)
#### [Déployer le service Mobilité avec Azure Automation DSC](site-recovery-automate-mobility-service-install.md)
### Activer la réplication
#### [Activer la réplication Azure vers Azure](site-recovery-replicate-azure-to-azure.md)
#### [Activer la réplication VMware vers Azure](site-recovery-replicate-vmware-to-azure.md)
## Basculement et restauration automatique
### [Configurer des plans de récupération](site-recovery-create-recovery-plans.md)
#### [Ajouter des Runbooks Azure à des plans de récupération](site-recovery-runbook-automation.md)
### Exécution d’un test de basculement
#### [Exécution d’un test de basculement vers Azure](site-recovery-test-failover-to-azure.md)
#### [Exécuter un test de basculement entre des clouds VMM](site-recovery-test-failover-vmm-to-vmm.md)
### [Basculer des machines protégées](site-recovery-failover.md)
### Restaurer la protection des machines après un basculement
#### [Protéger de nouveau à partir d’une région secondaire Azure vers une région primaire](site-recovery-how-to-reprotect-azure-to-azure.md)
#### [Reprotection d’Azure vers votre site local](site-recovery-how-to-reprotect.md)
### Effectuer une restauration automatique à partir d’Azure
#### [Restauration automatique de Microsoft Azure vers VMware](site-recovery-failback-azure-to-vmware.md)
#### [Restauration automatique de Microsoft Azure vers Hyper-V](site-recovery-failback-from-azure-to-hyper-v.md)
## Migrer
### [Migration vers Azure](site-recovery-migrate-to-azure.md)
### [Migrer entre des régions Azure](site-recovery-migrate-azure-to-azure.md)
### [Migrer des instances Windows AWS dans Azure](site-recovery-migrate-aws-to-azure.md)
### [Répliquer des ordinateurs migrés sur une autre région Azure](site-recovery-azure-to-azure-after-migration.md)
## Charges de travail
### [Active Directory et DNS](site-recovery-active-directory.md)
### [Réplication de SQL Server](site-recovery-sql.md)
### [SharePoint](site-recovery-sharepoint.md)
### [Dynamics AX](site-recovery-dynamicsax.md)
### [RDS](site-recovery-workload.md#protect-rds)
### [Microsoft Exchange](site-recovery-workload.md#protect-exchange)
### [SAP](site-recovery-workload.md#protect-sap)
### [Applications web basées sur IIS](site-recovery-iis.md)
### [Citrix XenApp et XenDesktop](site-recovery-citrix-xenapp-and-xendesktop.md)
### [Autres charges de travail](site-recovery-workload.md#workload-summary)
## Automatiser la réplication
### [Automatiser la réplication Hyper-V dans Azure (sans VMM)](site-recovery-deploy-with-powershell-resource-manager.md)
### [Automatiser la réplication Hyper-V dans Azure (avec VMM)](site-recovery-vmm-to-azure-powershell-resource-manager.md)
### [Automatiser la réplication Hyper-V vers un site secondaire (avec VMM)](site-recovery-vmm-to-vmm-powershell-resource-manager.md)
## Gérer
### [Gérer des serveurs de processus dans Azure](site-recovery-vmware-setup-azure-ps-resource-manager.md)
### [Gérer le serveur de configuration](site-recovery-vmware-to-azure-manage-configuration-server.md)
### [Gérer des serveurs de processus de montée en puissance parallèle](site-recovery-vmware-to-azure-manage-scaleout-process-server.md)
### [Gérer des serveurs vCenter](site-recovery-vmware-to-azure-manage-vCenter.md)
### [Supprimer des serveurs et désactiver la protection](site-recovery-manage-registration-and-protection.md)

## Surveiller et résoudre des problèmes
### [Problèmes de réplication Azure vers Azure](site-recovery-azure-to-azure-troubleshoot-errors.md)
### [Problèmes de réplication d’un site local vers Azure](site-recovery-vmware-to-azure-protection-troubleshoot.md)
### [Collecter les journaux et résoudre les problèmes locaux](site-recovery-monitoring-and-troubleshooting.md)

# Référence
## [PowerShell](/powershell/module/azurerm.siterecovery)
## [PowerShell Classic](/powershell/module/azure/?view=azuresmps-3.7.0)
## [REST](https://msdn.microsoft.com/en-us/library/mt750497)

# Rubriques connexes
## [Azure Automation](/azure/automation/)

# les ressources
## [Parcours d’apprentissage](https://azure.microsoft.com/documentation/learning-paths/site-recovery/)
## [Forum](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hypervrecovmgr)
## [Blog](http://azure.microsoft.com/blog/tag/azure-site-recovery/)
## [Tarification](https://azure.microsoft.com/pricing/details/site-recovery/)
## [Mises à jour de service](https://azure.microsoft.com/updates/?product=site-recovery)
