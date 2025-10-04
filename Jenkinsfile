pipeline {
    agent none

    tools {
        maven 'M3' 
    }

    stages {
        stage('Test') {
            agent { label 'test' } 
            steps {
                echo 'Building and testing application on test node...'
                sh 'mvn clean install'
                stash includes: 'target/*.jar', name: 'app-artifact'
            }
        }

        stage('Deploy to Prod') {
            agent { label 'prod' } 
            when {
                expression { currentBuild.result == 'SUCCESS' }
            }
            steps {
                echo 'Deploying to production node...'
                unstash 'app-artifact'
                sh 'scp -i /var/lib/jenkins/.ssh/id_rsa target/*.jar ec2-user@13.218.106.218:/home/ec2-user/app.jar'
            }
        }
    }
}
