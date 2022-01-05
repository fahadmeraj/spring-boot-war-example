pipeline{
    agent any
    tools {
        maven 'MAVEN'
    }
    stages{
        stage("Test"){
            steps{
                slackSend channel: 'mavenproject-jenkins', message: 'job started'
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
                deploy adapters: [tomcat9(credentialsId: 'merajcredentials', path: '', url: 'http://18.223.122.71:8080')], contextPath: '/app', war: '**/*.war'
            }
        }
        stage("Deploy on Prod"){
            input{
                message 'should we continue?'
                ok 'yes we should'
            }
            steps{
                deploy adapters: [tomcat9(credentialsId: 'merajcredentials', path: '', url: 'http://18.222.219.152:8080')], contextPath: '/app', war: '**/*.war'
            }
        }
    }
    post{
        always{
            echo "========always========"
        }
        success{
            echo "========pipeline executed successfully ========"
            slackSend channel: 'mavenproject-jenkins', message: 'job succeeded'
        }
        failure{
            echo "========pipeline execution failed========"
            slackSend channel: 'mavenproject-jenkins', message: 'job failed'
        }
    }
}
