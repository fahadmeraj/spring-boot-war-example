pipeline{
    agent any
     tools {
        maven 'Maven' 
    }
    stages{
        stage("Test"){
            steps{
                slackSend channel: 'mavenproject-jenkins', message: 'job started'
                sh 'mvn test'
            }   
        }
        stage("Build"){
            steps{
                sh 'mvn package'
            }   
        }
        stage("Deploy on test"){
            steps{
                deploy adapters: [tomcat9(credentialsId: '21f07f9c-f662-4988-bd2f-c8ec467ec4a3', path: '', url: 'http://192.168.1.140:8090')], contextPath: '/app2', war: '**/*.war'
            }   
        }
        stage("Deploy on prod"){
            input {
                message 'should we continue?'
                ok 'yes we should...'
            }
            steps{
                deploy adapters: [tomcat9(credentialsId: '98bfe940-4d24-4d26-af36-2860afb1a2bc', path: '', url: 'http://18.191.216.49:8080')], contextPath: '/app2', war: '**/*.war'
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
