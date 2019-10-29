pipeline {
    agent {
        docker {
            image 'maven:3-alpine'
            args '-v /root/.m2:/root/.m2'
        }
    }
    node {
        git url: 'https://github.com/gramundi/simple-java-maven-app.git'
        input 'Ready to go?'
        
    }
    stages {
        stage('Build') {
            //when { anyOf { branch 'master'; branch 'development' } }
            steps {
                echo env
                sh 'mvn -B clean package sonar:sonar'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test sonar:sonar'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('Deliver') {
            steps {
                sh './jenkins/scripts/deliver.sh'
            }
        }
    }

}

def getGitBranchName() {
    return scm.branches[0].name
}