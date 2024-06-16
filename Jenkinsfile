pipeline {
    agent any
    
    environment {
        DATABASE_URL = 'jdbc:mysql://18.234.36.210:3306/db1'
    }
    
    stages {
        stage('Checkout') {
            steps {
                git credentialsId: 'github-token', url: 'https://github.com/Bairavir/your-repository.git', branch: 'main'
            }
        }
        stage('Apply Database Changes') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'db1', passwordVariable: 'DB_PASSWORD', usernameVariable: 'DB_USERNAME')]) {
                    script {
                        echo 'Deploying SQL...'
                        sh '''
                            echo "Database URL: ${DATABASE_URL}"
                            echo "Database Username: ${DB_USERNAME}"
                            echo "Running Liquibase Update..."
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
