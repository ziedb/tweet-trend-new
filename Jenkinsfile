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
        stage("test"){
            steps{
                echo "------------ test started ---------"
                sh 'mvn surefire-report:report'
                echo "------------ test completed ---------"
            }
        }

        stage('SonarQube analysis'){
        environment{
            scannerHome = tool 'sonar-scanner'
        }
        steps{
            echo '<--------------- Sonar Analysis started  --------------->'
            withSonarQubeEnv('sonarqube-server') {
                sh '${scannerHome}/bin/sonar-scanner'
            echo '<--------------- Sonar Analysis stopped  --------------->'
            }
        }    
        }
        stage("Quality Gate"){
            steps{
                echo '<--------------- Sonar Quality Gate Analysis started  --------------->'
                script{
                        timeout(time: 1, unit: 'HOURS') { // Just in case something goes wrong, pipeline will be killed after a timeout
                        def qg = waitForQualityGate() // Reuse taskId previously collected by withSonarQubeEnv
                        if (qg.status != 'OK') {
                        error "Pipeline aborted due to quality gate failure: ${qg.status}"
                        }
                        }
                    }
                echo '<--------------- Sonar Quality Gate Analysis stopped  --------------->'
            }
        }

    }


}
