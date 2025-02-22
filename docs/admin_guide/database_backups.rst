Database Backups
================

The Importance of Backups
-------------------------

Drupal, on which Tripal is based, is not like a word processor or spreadsheet, there is no "Undo" function. For this reason, and to guard against hard drive failures, etc. it is essential to maintain current backups of your PostgreSQL database.

How to make a backup
--------------------

You can use the PostgreSQL command ``pg_dump`` to make a backup of your database. Here is a simple script that makes a database backup, and saves it in a file which is named using today's date. You would of course substitute your site's hostname, database name, file path, username, and password.

.. code-block:: bash

  #!/bin/bash

  file="/var/www/drupal9/web/$(date +%Y%m%d).sql"
  echo "Backup Tripal database to \"$file\""

  PGPASSWORD=drupaldevelopmentonlylocal \
  pg_dump \
    --verbose \
    --format=plain \
    --username="drupaladmin" \
    --host=localhost \
    "sitedb" \
    > "$file"

You could then run this script daily to make sure you always have a current database backup. One way to do that is to use cron on your system. Although setting that up is beyond the scope of this documentation, you may find `this tutorial by GeeksForGeeks <https://www.geeksforgeeks.org/crontab-in-linux-with-examples/>`_ helpful.

How to restore from a backup
----------------------------

For convenience we can first specify the PostgreSQL password once so we don't need to enter it for each step. You may prefer a more secure `authentication method <https://www.postgresql.org/docs/current/auth-methods.html>`_, but for learning Tripal on a local system this is the simplest method. Substitute your password here.

.. code-block:: bash

  export PGPASSWORD=drupaldevelopmentonlylocal

In order to restore from a backup, we first need to remove the current database, and create a new empty database. **This will erase all data**, so make sure you successfully made a backup first!

.. code-block:: bash

  dropdb \
    --username="drupaladmin" \
    --host=localhost \
    --if-exists sitedb

  createdb \
    --username="drupaladmin" \
    --host=localhost \
    sitedb -O drupaladmin

Restoring data into the database is then performed as follows, use the file name generated by the previous backup step, for example it might be ``20240506.sql``.

.. code-block:: bash

  psql \
    --set ON_ERROR_STOP=on \
    --username="drupaladmin" \
    --host=localhost \
    --dbname=sitedb \
    < substitutefilenamehere.sql

While not essential, for your convenience you may want to place chado in the default search path. To do that, execute this command:

.. code-block:: bash

  psql -h localhost -U drupaladmin -d sitedb \
    -c "ALTER DATABASE sitedb SET search_path = '$user', public, chado"

Finally, it is recommended to rebuild the Drupal caches

.. code-block:: bash

  drush cache:rebuild

Best Practices
--------------

If you are just learning Tripal, we recommend you start out with a :ref:`Tripal Docker` container. This makes initial installation as easy as possible, and if you make mistakes with your site, it is easy to start over with a new clean starting point. You can also backup and restore the database inside your docker container as described earlier. Another approach when using docker is to use `docker commit <https://docs.docker.com/reference/cli/docker/container/commit/>`_ to create an image from a running container at the point you want to save. Then you can use the docker run command with this committed image in order to start a fresh site at the exact same point.

If you have a publicly facing web site, which we usually call a "Production" site, it is highly recommended to also have a "Staging" or "Testing" site. Here you can load a database backup from your production site, and then test new loaders or procedures on the staging site without danger of harming your production site. Once your procedures are verified as working correctly, only then do you make changes to your production site.
