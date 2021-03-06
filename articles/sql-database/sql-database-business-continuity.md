<properties
   pageTitle="Cloud business continuity - database recovery - SQL Database | Microsoft Azure"
   description="Learn how Azure SQL Database supports cloud business continuity and database recovery and helps keep mission-critical cloud applications running."
   keywords="business continuity,cloud business continuity,database disaster recovery,database recovery"
   services="sql-database"
   documentationCenter=""
   authors="elfisher"
   manager="jhubbard"
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="sqldb-business-continuity"
   ms.date="06/09/2016"
   ms.author="elfish"/>

# Overview: Cloud business continuity and database disaster recovery with Azure SQL Database

Azure SQL Database provides a number of business continuity solutions. Business continuity is about designing, deploying, and running applications in a way that is resilient to planned or unplanned disruptive events that result in permanent or temporary loss of the application’s ability to conduct its business function. Unplanned events range from human errors to permanent or temporary outages to regional disasters that could cause wide scale loss of facility in a particular Azure region. The planned events include application redeployment to a different region and application upgrades. The goal of business continuity is for your application to continue to function during these events with minimal impact on the business function.

To discuss SQL Database cloud business continuity solutions, there are several concepts with which you need be familiar. These are:

* **Disaster recovery (DR):** a process of restoring the normal business function of the application

* **Estimated Recovery Time (ERT):** The estimated duration for the database to be fully available after a restore or failover request.

* **Recovery time objective (RTO)**: The maximum acceptable time before the application fully recovers after the disruptive event. RTO measures the maximum loss of availability during the failures.

* **Recovery point objective (RPO)**: The maximum amount of last updates (time interval) the application can lose by the moment it fully recovers after the disruptive event. RPO measures the maximum loss of data during the failures.


## SQL Database cloud business continuity scenarios

The key scenarios to consider when planning for business continuity and database recovery are discussed below.

### Design applications for business continuity

The application I am building is critical for my business. I want to design and configure it to be able to survive a regional disaster of catastrophic failure of the service. I know the RPO and RTO requirements for my application and will choose the configuration that meets these requirements.

### Recover from human error

I have administrative rights to access the production version of the application. As part of regular maintenance process I made a mistake and deleted some critical data in production. I want to quickly restore the data in order to mitigate the impact of the error.

### Recover from an outage

I am running my application in production and receive an alert suggesting that there is a major outage in the region it is deployed in. I want to initiate the recovery process to bring it back in a different region to mitigate the impact on the business.

### Perform disaster recovery drill

Because the recovery from an outage will relocate the application’s data tier to a different region I want to periodically test the recovery process and evaluate its impact on the application to stay prepared.

### Application upgrade without downtime

I am releasing a major upgrade of my application. It involves the database schema changes, deployment of additional stored procs etc. This process will require stopping user access to the database. At the same I want to make sure the upgrade does not cause a significant disruption of the business operations.

## SQL Database business continuity features

The following table lists the SQL Database business continuity features and shows their differences across the [service tiers](sql-database-service-tiers.md):

| Capability | Basic tier | Standard tier |Premium tier
| --- |--- | --- | ---
| Point In Time Restore | Any restore point within 7 days | Any restore point within 14 days | Any restore point within 35 days
| Geo-Restore | ERT < 12h, RPO < 1h | ERT < 12h, RPO < 1h | ERT < 12h, RPO < 1h
| Active Geo-Replication | ERT < 30s, RPO < 5s | ERT < 30s, RPO < 5s | ERT < 30s, RPO < 5s

These features are provided to address the scenarios listed earlier. Please refer to the [Design for business continuity](sql-database-business-continuity-design.md) section for guidance how to select the specific feature.

> [AZURE.NOTE] The ERT and RPO values are engineering goals and provide guidance only. They are not part of [SLA for SQL Database](https://azure.microsoft.com/support/legal/sla/sql-database/v1_0/)


###Point In Time Restore

[Point In Time Restore](sql-database-point-in-time-restore.md) is designed to return your database to an earlier point in time. It uses the database backups, incremental backups and transaction log backups that the service automatically maintains for every user database. This capability is available for  all service tiers. You can go back 7 days with Basic, 14 days with Standard, and 35 days with Premium. Refer to [Recover from human error](sql-database-user-error-recovery.md) for details of how to use Point In Time Restore.

### Geo-Restore

[Geo-Restore](sql-database-geo-restore.md) is also available with Basic, Standard, and Premium databases. It provides the default recovery option when also  database is unavailable because of an incident in the region where your database is hosted. Similar to Point In Time Restore, Geo-Restore relies on database backups in geo-redundant Azure storage. It restores from the geo-replicated backup copy and therefore is resilient to the storage outages in the primary region.  Refer to [Recover from an outage](sql-database-disaster-recovery.md) for details of how to use Geo-Restore.

### Active Geo-Replication

[Active Geo-Replication](sql-database-geo-replication-overview.md) is available for all database tiers. It’s designed for applications that have more aggressive recovery requirements than Geo-Restore can offer. Using Active Geo-Replication, you can create up to four readable secondaries on servers in different regions. You can initiate failover to any of the secondaries.  In addition, Active Geo-Replication can be used to support the application upgrade or relocation scenarios, as well as load balancing for read-only workloads. Refer to [Design for business continuity](sql-database-business-continuity-design.md) for details on how to [configure Geo-Replication](sql-database-geo-replication-portal.md) and to [failover to the secondary database](sql-database-geo-replication-failover-portal.md). Refer to [Application upgrade without downtime](sql-database-business-continuity-application-upgrade.md) for details on how to implement the application upgrade without downtime.

## Next Steps

- [Design for business continuity](sql-database-business-continuity-design.md)
- [Point In Time Restore](sql-database-point-in-time-restore.md)
- [Geo-Restore](sql-database-geo-restore.md)
- [Active Geo-Replication](sql-database-geo-replication-overview.md)


## Additional Resources

- [SQL Database automated backups](sql-database-automated-backups.md)
- [Application upgrade without downtime](sql-database-business-continuity-application-upgrade.md)