Release RESThub
===============

Instructions provided with 2.0-beta1. Adapt with your version (search and replace)

Prepare
-------

	* Clone all projects from https://github.com/resthub
	* Check that you have in your settings.xml the configuration for Maven repsotiroy passwords

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
	* cd resthub-backbone-stack
	* mvn versions:set -DnewVersion=2.0-beta1 -DgenerateBackupPoms=false
	* mvn clean install deploy
	* git commit -a -m "Change version to 2.0-beta1"
	* git tag resthub-backbone-stack-2.0-beta1
	* mvn versions:set -DnewVersion=2.0-SNAPSHOT -DgenerateBackupPoms=false
	* git commit -a -m "Change version to 2.0-SNAPSHOT"
	* git push
	* git push --tags

Release resthub-spring-stack :	
	* cd ../resthub-spring-stack
	* mvn versions:set -DnewVersion=2.0-beta1 -DgenerateBackupPoms=false
	* mvn clean install deploy
	* git commit -a -m "Change version to 2.0-beta1"
	* git tag resthub-spring-stack-2.0-beta1
	* mvn versions:set -DnewVersion=2.0-SNAPSHOT -DgenerateBackupPoms=false
	* git commit -a -m "Change version to 2.0-SNAPSHOT"
	* git push
	* git push --tags

Release resthub-archetypes :	
	* cd ../resthub-archetypes
	* mvn versions:set -DnewVersion=2.0-beta1 -DgenerateBackupPoms=false
	* mvn clean install
	* mvn deploy
	* git commit -a -m "Change version to 2.0-beta1"
	* git tag resthub-archetypes-2.0-beta1
	* mvn versions:set -DnewVersion=2.0-SNAPSHOT -DgenerateBackupPoms=false
	* git commit -a -m "Change version to 2.0-SNAPSHOT"
	* git push
	* git push --tags

Release todo-example :	
	* cd ../todo-example
	* mvn versions:set -DnewVersion=2.0-beta1 -DgenerateBackupPoms=false
	* mvn clean install deploy
	* git commit -a -m "Change version to 2.0-beta1"
	* git tag todo-example-2.0-beta1
	* mvn versions:set -DnewVersion=2.0-SNAPSHOT -DgenerateBackupPoms=false
	* git commit -a -m "Change version to 2.0-SNAPSHOT"
	* git push
	* git push --tags

Release documentation :
	* cd ../resthub.org
	* Manually update version and release properties in conf.py
	* Search and replace old version by new version in spring-stack.rst
	* git commit -a -m "Change version to 2.0-beta1"
	* git tag 2.0-beta1
	* git push
	* git push --tags

	

