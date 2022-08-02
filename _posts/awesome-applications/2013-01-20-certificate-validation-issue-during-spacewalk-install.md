---
categories:
  - awesome-applications
  - spacewalk
tags: centos
layout: post
title: Certificate validation issue during Spacewalk install
created: 1358656451
---

### Error

For some really annoying reason Spacewalk failed to populate the database during the initial setup.

```bash
[root@spacewalk ~]# spacewalk-setup --disconnected --external-db
** Database: Setting up database connection for PostgreSQL backend.
Hostname (leave empty for local)? 
Database? dbnamehere
Username? usernamehere
Password? 
** Database: Populating database.
The Database has schema.  Would you like to clear the database [Y]? Y
** Database: Clearing database.
** Database: Shutting down spacewalk services that may be using DB.
** Database: Services stopped.  Clearing DB.
** Database: Re-populating database.
*** Progress: ##################################
* Setting up users and groups.
** GPG: Initializing GPG and importing key.
* Performing initial configuration.
* Activating Spacewalk.
** Loading Spacewalk Certificate.
** Verifying certificate locally.
** Activating Spacewalk.
There was a problem validating the satellite certificate: 1
```

### Fix

Make sure your user's database password does not have special characters!
