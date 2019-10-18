**Understand version control of liquibase**

Suppose at beginning we have a clean database with nothing in it. The database change log has 3 versions, 1.0, 1,1 and 1.2 defined in previouse chapter. latest version is 1.2.

version 1.0, create 2 tables

version 1.1, change column name of table department

version 1.2, add foreign key and index

1) Creating database schema to current latest version:

       _____liquibase  --defaultsFile=src/main/resources/db/changelog/liquibase/liquibase.properties \
           --classpath="d:\mysql-connector-java-5.1.6.jar;/home/gibran/Documents/liquibase-tutorial/target/liquibase-helloworld-demo-0.0.1-SNAPSHOT.jar"  \
           --changeLogFile=liquibase/db-changelog-master.xml   \
           update_____

        a) The defaultsFile specify the location of properties file for database connection. 

        b) classpath specify where to find all necessary java file and xml change log files. Here are 2 jar files, one is mysql or h2 jdbc driver, the other is the jar of our demo, from which to read the changelog xml file.

        c) changeLogFile specify the file name of change log.
  
        d) update is the command for liquibase, to update database according to the xml change log file.
        
        The following maven command is equivalent to the previous command line.
            
            ~~mvn liquibase:update~~ 

2) For some reason you want to roll back you database to verion 1.0.  You can achieve that by command line:

       _liquibase  --defaultsFile=src/main/resources/liquibase/liquibase.properties \
           --classpath="d:\mysql-connector-java-5.1.6.jar;/home/gibran/Documents/liquibase-tutorial/target/liquibase-helloworld-demo-0.0.1-SNAPSHOT.jar"  \
           --changeLogFile=liquibase/db-changelog-master.xml   \
           rollback 1.0_
           
        Maven command: mvn liquibase:rollback -Dliquibase.rollbackTag=1.0 
        
3) Now the database is in status 1.0, and you want to apply 1.1 to it. you can do that by following command.

       liquibase  --defaultsFile=src/main/resources/liquibase/liquibase.properties \
           --classpath="d:\mysql-connector-java-5.1.6.jar;/home/gibran/Documents/liquibase-tutorial/target/liquibase-helloworld-demo-0.0.1-SNAPSHOT.jar"  \
           --changeLogFile=liquibase/db-changelog-master.xml   \
           updateToTag 1.1
           
        Maven commad: mvn liquibase:update -Dliquibase.toTag=1.1            
           
4) If you already have everything configured in database by hand or sql. You can use liqubase to generate change log file for you, then you can keep working based on the generated xml.

       liquibase  --defaultsFile=src/main/resources/liquibase/liquibase.properties \
           --classpath="d:\mysql-connector-java-5.1.6.jar;/home/gibran/Documents/liquibase-tutorial/target/liquibase-helloworld-demo-0.0.1-SNAPSHOT.jar"  \
           --changeLogFile=/output.xml   \
           generateChangeLog
           
        **NOTE: The changeLogFile is a filename to be created.  The file name must end with ".xml", ".json" or ".yaml".**
        
        Maven Command: mvn liquibase:generateChangeLog -Dliquibase.outputChangeLogFile=/output.xml

5) Reference: http://shengwangi.blogspot.com/2016/04/liquibase-helloworld-example.html                   