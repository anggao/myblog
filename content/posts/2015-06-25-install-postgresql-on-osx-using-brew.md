---
title: "Install PostgreSQL on OSX using brew"
date: "2015-06-25"
template: "post"
draft: false
slug: "install-postgresql-on-osx-using-brew"
category: "postgres"
tags:
  - "postgres"
description: ""
socialImage: ""
---

- Install PostgreSQL using brew If you don’t have homebrew, install it first. Then simply run the command:

```shell
brew install postgres
```

- Initialize PostgreSQL

```shell
initdb ~/pg-data
```

- Start PostgreSQL service

```shell
postgres -D ~/pg-data
```

- Create a user

```shell
# If you wish to create a user without a password just take the --pwprompt off the command.
createuser --pwprompt username
```

- Create a database

```shell
# The -O indicates the user that will become the owner of the database.
createdb -Ousername -Eutf8 dev-db
```

- Access the Database

```shell
# The -U means to login using that username and the -W means to prompt for a password.
psql -U username -W dev-db
```

- Remote Connection to PostgreSQL Database using psql

```shell
# edit pg_hba.conf at pg-data
host    all         all         192.168.1.10/24    trust
```

This will allow connection from 192.168.1.10 ip-address. If you want to allow connection from multiple client machines on a specific network, specify the network address here in the CIDR-address format.
