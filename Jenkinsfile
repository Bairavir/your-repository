pipeline {
    agent any
    
    environment {
        DATABASE_URL = 'jdbc:mysql://18.234.36.210:3306/mydatabase' // Adjust this to your database URL
    }
    
    stages {
        stage('Checkout') {
            steps {
                git credentialsId: 'github-token', url: 'https://github.com/bairavir/your-repository.git'
            }
        }
        stage('Apply Database Changes') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'db1', passwordVariable: 'DB_PASSWORD', usernameVariable: 'DB_USERNAME')]) {
                    script {
                        echo 'Deploying SQL...'
                        sh '''
                            /usr/local/bin/liquibase \
                            --url=${DATABASE_URL} \
                            --username=${DB_USERNAME} \
                            --password=${DB_PASSWORD} \
                            --changeLogFile=sql/changelog.xml update
                        '''
                    }
                }
            }
        }
    }
    post {
        failure {
            echo 'Database update failed!'
        }
    }
}
