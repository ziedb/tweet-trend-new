pipeline{
    agent{
        node{
            label 'maven'
        }
    }
environment{
    PATH = "/opt/apache-maven-3.9.9/bin:$PATH"
}
    stages {
        stage("buid"){
            steps{
                echo "------------ build started ---------"
                sh 'mvn clean deploy -Dmaven.test.skip=true'
                echo "------------ build completed ---------"
            }
        }

        stage('SonarQube analysis'){
        environment{
            scannerHome = tool 'sonar-scanner'
        }
        steps{
            echo '<--------------- Sonar Analysis started  --------------->'
            withSOnarQubeEnv('sonarqube-server') {
                sh '${scannerHome}/bin/sonar-scanner'
            echo '<--------------- Sonar Analysis stopped  --------------->'
            }
        }    
        }
    }





}
