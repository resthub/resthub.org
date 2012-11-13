Release RESThub
===============

Instructions provided with 2.0-rc4. Adapt with your version (search and replace)

Prepare
-------

    * Clone all projects from https://github.com/resthub
    * Check that you have in your settings.xml the configuration for Maven repsotiroy passwords

.. code-block:: xml

    <servers>
        <server>
          <id>pullrequest-releases</id>
          <username>admin</username>
          <password>XXXXXXXXXXX</password>
        </server>
          <server>
          <id>pullrequest-snapshots</id>
          <username>admin</username>
          <password>XXXXXXXXXXX</password>
        </server>
    </servers>

Release resthub-backbone-stack :

.. code-block:: bash

    cd resthub-backbone-stack
    git status
    git pull resthub master
    mvn versions:set -DnewVersion=2.0-rc4 -DgenerateBackupPoms=false
    mvn clean install
    mvn deploy -DaltDeploymentRepository=pullrequest-releases::default::http://nexus.pullrequest.org/content/repositories/releases
    git commit -a -m "Change version to 2.0-rc4"
    git tag resthub-2.0-rc4
    mvn versions:set -DnewVersion=2.0-SNAPSHOT -DgenerateBackupPoms=false
    git commit -a -m "Change version to 2.0-SNAPSHOT"
    git push resthub master
    git push resthub master --tags

Release resthub-spring-stack :

.. code-block:: bash

    cd ../resthub-spring-stack
    git status
    git pull resthub master
    mvn versions:set -DnewVersion=2.0-rc4 -DgenerateBackupPoms=false
    mvn clean install
    mvn deploy -DaltDeploymentRepository=pullrequest-releases::default::http://nexus.pullrequest.org/content/repositories/releases
    git commit -a -m "Change version to 2.0-rc4"
    git tag resthub-2.0-rc4
    mvn versions:set -DnewVersion=2.0-SNAPSHOT -DgenerateBackupPoms=false
    git commit -a -m "Change version to 2.0-SNAPSHOT"
    git push resthub master
    git push resthub master --tags

Release resthub-archetypes :

.. code-block:: bash

    cd ../resthub-archetypes
    git status
    git pull resthub master
    mvn versions:set -DnewVersion=2.0-rc4 -DgenerateBackupPoms=false
    mvn clean install
    mvn -DskipTests=true deploy -DaltDeploymentRepository=pullrequest-releases::default::http://nexus.pullrequest.org/content/repositories/releases
    git commit -a -m "Change version to 2.0-rc4"
    git tag resthub-2.0-rc4
    mvn versions:set -DnewVersion=2.0-SNAPSHOT -DgenerateBackupPoms=false
    git commit -a -m "Change version to 2.0-SNAPSHOT"
    git push resthub master
    git push resthub master --tags

Release todo-example :

.. code-block:: bash

    cd ../todo-example
    git status
    git pull resthub master
    mvn versions:set -DnewVersion=2.0-rc4 -DgenerateBackupPoms=false
    mvn clean install
    mvn deploy -DaltDeploymentRepository=pullrequest-releases::default::http://nexus.pullrequest.org/content/repositories/releases
    git commit -a -m "Change version to 2.0-rc4"
    git tag resthub-2.0-rc4
    mvn versions:set -DnewVersion=2.0-SNAPSHOT -DgenerateBackupPoms=false
    git commit -a -m "Change version to 2.0-SNAPSHOT"
    git push
    git push --tags

Release documentation :

.. code-block:: bash

    cd ../resthub.org
    git status
    git pull resthub master
    [BE CAREFUL] Manually update version and release properties in conf.py
    Search and replace old version by new version in spring-stack.rst
    git commit -a -m "Change version to 2.0-rc4"
    git tag resthub-2.0-rc4
    git push resthub master
    git push resthub master --tags
    

