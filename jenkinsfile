pipeline{
    agent any
    tools {
        maven 'MAVEN'
    }
    stages{
        stage("Test"){
            steps{
                bat 'mvn --version'
                bat 'mvn test'
            }
        }
        stage("Deploy"){
            steps{
                bat 'mvn package'
            }
        }
        stage("Deploy on Test"){
            steps{
                deploy adapters: [tomcat9(credentialsId: 'merajcredentials', path: '', url: 'http://3.144.121.232:8080')], contextPath: '/app', war: '**/*.war'
            }
        }
        stage("Deploy on Prod"){
            steps{
                deploy adapters: [tomcat9(credentialsId: 'merajcredentials', path: '', url: 'http://3.144.121.232:8080')], contextPath: '/app', war: '**/*.war'
            }
        }
    }
    post{
        always{
            echo "========always========"
        }
        success{
            echo "========pipeline executed successfully ========"
        }
        failure{
            echo "========pipeline execution failed========"
        }
    }
}
