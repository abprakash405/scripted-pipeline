node {
    def mvnHome
    stage('CODE') { // for display purposes
        // Get some code from a GitHub repository
        git 'https://github.com/abprakash405/maven-war.git'
        // Get the Maven tool.
        // ** NOTE: This 'M3' Maven tool must be configured
        // **       in the global configuration.
        mvnHome = tool 'maven-3'
    }
    stage('Build') {
        // Run the maven build
        withEnv(["MVN_HOME=$mvnHome"]) {
            if (isUnix()) {
                sh '"$MVN_HOME/bin/mvn" -Dmaven.test.failure.ignore clean package'
            } else {
                bat(/"%MVN_HOME%\bin\mvn" -Dmaven.test.failure.ignore clean package/)
            }
        }
    }
    stage('DEPLOY') {
           //delete old package if required
           bat 'curl "http://admin:admin@localhost:8082/manager/text/undeploy?path=/asn-war-example-1.0.0-SNAPSHOT"'
           
           bat 'curl --upload-file target/asn-war-example-1.0.0-SNAPSHOT.war "http://admin:admin@localhost:8082/manager/text/deploy?path=/asn-war-example-1.0.0-SNAPSHOT"'
    }
}
