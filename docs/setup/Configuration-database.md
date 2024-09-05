# Configuration - Database Settings

The CSE supports different types of databases. The database settings are configured in the configuration file under the section `[database]` and its subsections.


##	General Settings

**Section: `[database]`**

These are the general database settings.

| Setting        | Description                                                                                                                                        | Default                                                                                               |
|:---------------|:---------------------------------------------------------------------------------------------------------------------------------------------------|:------------------------------------------------------------------------------------------------------|
| backupPath     | The directory for a backup of the database files.<br />Database backups are not supported for the in-memory database and postgreSQL.               | [${basic.config:baseDirectory}](../setup/Configuration-introduction.md#built-in-settings)/data/backup |
| resetOnStartup | Reset the databases at startup.<br/>See also command line argument [--db-reset](../setup/Running.md).                                              | False                                                                                                 |
| type           | The type of database to use.<br />See also command line argument [--db-type](../setup/Running.md).<br />Allowed values: tinydb, postgresql, memory | tinydb                                                                                                |


## TinyDB

**Section: `[database.tinydb]`**

These are the settings for the TinyDB database. The *cacheSize* and *writeDelay* settings are only used if the database type is set to *tinydb* (ie. in file-based mode). They have a major impact on the performance of the database.

| Setting    | Description                                                                                  | Default                                                                                        |
|:-----------|:---------------------------------------------------------------------------------------------|:-----------------------------------------------------------------------------------------------|
| cacheSize  | Cache size in bytes, or 0 to disable caching.                                                | 0                                                                                              |
| path       | Directory for the database files.                                                            | [${basic.config:baseDirectory}](../setup/Configuration-introduction.md#built-in-settings)/data |
| writeDelay | Delay in seconds before new data is written to disk to avoid trashing. Must be full seconds. | 1 second                                                                                       |


## PostgreSQL

**Section: `[database.postgresql]`**

These are the settings for the PostgreSQL database. 

| Setting  | Description                              | Default                                                                      |
|:---------|:-----------------------------------------|:-----------------------------------------------------------------------------|
| database | Name of the database.                    | [${basic.config:cseID}](../setup/Configuration-basic.md#basic-configuration) |
| host     | Hostname of the PostgreSQL server.       | localhost                                                                    |
| password | Password for the database.               | not set                                                                      |
| port     | Port of the PostgreSQL server.           | 5432                                                                         |
| schema   | Name of the schema.<br/>Default: acmecse | acmecse                                                                      |
| role     | Login/Username for the database.         | [${basic.config:cseID}](../setup/Configuration-basic.md#basic-configuration) |
