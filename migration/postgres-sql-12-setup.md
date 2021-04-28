---
description: Petrus' experience to setup postgresql
---

# Postgres SQL 12 setup

## Starting Resource

The below is a great resource for a start.

{% hint style="info" %}
[https://www.robinwieruch.de/postgres-sql-macos-setup](https://www.robinwieruch.de/postgres-sql-macos-setup)
{% endhint %}

**Do Remember**

To use postgresql version 12.5, use the below instead of a plain `postgresql` install

```text
 brew install postgresql@12
```

## Caveats to Installing postgresql@12

The terminal output the following information:  


> This formula has created a default database cluster with:

> initdb --locale=C -E UTF-8 /usr/local/var/postgresql@12
>
> For more details, read:
>
>   https://www.postgresql.org/docs/12/app-initdb.html
>
> postgresql@12 is keg-only, which means it was not symlinked into /usr/local,
>
> because this is an alternate version of another formula.
>
> If you need to have postgresql@12 first in your PATH, run:
>
>   echo 'export PATH="/usr/local/opt/postgresql@12/bin:$PATH"' &gt;&gt; ~/.zshrc
>
> For compilers to find postgresql@12 you may need to set:
>
>   export LDFLAGS="-L/usr/local/opt/postgresql@12/lib"
>
>   export CPPFLAGS="-I/usr/local/opt/postgresql@12/include"
>
> To have launchd start postgresql@12 now and restart at login:
>
>   brew services start postgresql@12
>
> Or, if you don't want/need a background service you can just run:
>
>   pg\_ctl -D /usr/local/var/postgresql@12 start

**Note:** Instead of the `/usr/local/var/postgres`  directory, the version 12 directory is setup as

```text
/usr/local/var/postgresql@12
```

and to start the service, you should run:

```text
pg_ctl -D /usr/local/var/postgresql@12 start
```

but this this does not work, even after I have add `postgres@12` to the path and exported the flags as per above.

{% hint style="danger" %}
zsh: command not found pg\_ctl
{% endhint %}

## Getting it Working

Restarting my Mac did the trick. Thereafter, `pg_ctl` ran 100%.

## Setting up the \`navlabtest\` Database

From here everything in the starting source tutorial worked fine including

```text
createdb navlabtest
```



## Additional Details for pgAdmin

```text
Installation Directory: /Library/PostgreSQL/13
Server Installation Directory: /Library/PostgreSQL/13
Data Directory: /Library/PostgreSQL/13/data
Database Port: 5432
Database Superuser: postgres
Operating System Account: postgres
Database Service: postgresql-13
Command Line Tools Installation Directory: /Library/PostgreSQL/13
pgAdmin4 Installation Directory: /Library/PostgreSQL/13/pgAdmin 4
Stack Builder Installation Directory: /Library/PostgreSQL/13

```



