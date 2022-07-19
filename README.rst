===============================================
Vulnerable Python Test
===============================================


Running
=======

Docker-compose
--------------

.. code-block :: bash

    docker-compose up

Then visit http://localhost:8080 in your favorite browser.

To rebuild the container, please use ``./recreate.sh`` script, which will
delete old container and create new from scratch. This script is primarly used
in order to rebuild application image.

If you have screwed up the database (i.e. with ``DROP TABLE students;``, please
issue the following commands to recreate database container:

.. code-block :: bash

    docker-compose stop postgres
    docker-compose rm  # make sure, you remove only images you want to recreate
    docker-compose up postgres  # recreate container and run

Requirements
~~~~~~~~~~~~

- Python3.6.2
- PostgreSQL database for data storage
- Redis for session storage

Installing and running
~~~~~~~~~~~~~~~~~~~~~~

.. code-block :: bash

    # Install application dependencies.
    pip install -r requirements.txt

    # Set up postgresql database Further I assume your db user
    # is named postgres and database name is sqli

    # Create database schema by applying migration 000
    psql -U postgres --d sqli --host localhot --port 5432 \
         -f migrations/000-init-schema.sql

    # Load fixtures into database
    psql -U postgres --d sqli --host localhot --port 5432 \
         -f migrations/001-fixtures.sql

    # Modify config/dev.yaml
    cat config/dev.yaml <<EOF
    db:
      user: postgres
      password: postgres
      host: localhost
      port: 5432
      database: sqli

    redis:
      host: localhost
      port: 6379
      db: 0

    app:
      host: 0.0.0.0
      port: 8080
    EOF

    # Run application
    python run.py

Then visit http://localhost:8080 in your favorite browser.


