Title: Postgres and postgis
Date: 2013-03-20 02:53
Author: alex
Category: Technology
Slug: postgres-and-postgis

I've recently have to set up postgres again and I always have to keep
looking up the commands for setting up a new user. On Ubuntu 13.04, I
installed postgresql and postgis using apt-get.

    sudo apt-get install postgresql python-dev libpq-dev postgresql-9.1-postgis postgresql-server-dev-9.1 python-psycopg2 python-software-properties

After installation, I had to create a template postgis database and set
up a user. I ran the following on the command line to set up the
database template.

    sudo su postgres -c'createdb -E UTF8 -U postgres template_postgis'
    sudo su postgres -c'createlang -d template_postgis plpgsql;'
    sudo su postgres -c'psql -U postgres -d template_postgis -c"CREATE EXTENSION hstore;"'
    sudo su postgres -c'psql -U postgres -d template_postgis -f /usr/share/postgresql/9.1/contrib/postgis-1.5/postgis.sql'
    sudo su postgres -c'psql -U postgres -d template_postgis -f /usr/share/postgresql/9.1/contrib/postgis-1.5/spatial_ref_sys.sql'
    sudo su postgres -c'psql -U postgres -d template_postgis -c"select postgis_lib_version();"'
    sudo su postgres -c'psql -U postgres -d template_postgis -c "GRANT ALL ON geometry_columns TO PUBLIC;"'
    sudo su postgres -c'psql -U postgres -d template_postgis -c "GRANT ALL ON spatial_ref_sys TO PUBLIC;"'
    sudo su postgres -c'psql -U postgres -d template_postgis -c "GRANT ALL ON geography_columns TO PUBLIC;"'

Next, log into psql as the postgres user and execute the following
commands while substituting the bracketed values:

    create database [DATABASE] template=template_postgis;
    create user [USER] with password '[PASSWORD]';
    grant all privileges on database [DATABASE] to [USER];
    alter user [USER] createdb;
