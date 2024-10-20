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

                sh 'mvn clean deploy'
            }
        }

        stage('SonarQube analysis'){
        environment{
            scannerHome = tool 'sonar-scanner'
        }
        steps{
            withSOnarQubeEnv('sonarqube-server') {
                sh '${scannerHome}/bin/sonar-scanner'
            }

        }    
        }
    }





}