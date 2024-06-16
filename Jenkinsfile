pipeline {
    agent any

    environment {
        DB_URL = 'jdbc:mysql://18.234.36.210:3306/db1'
        DB_USER = credentials('naruto') // Jenkins credentials ID for DB user
        DB_PASSWORD = credentials('12345678NARU') // Jenkins credentials ID for DB password
    }

    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/your_username/your_repository.git'
            }
        }

        stage('Run Liquibase') {
            steps {
                script {
                    sh """
                        /usr/local/bin/liquibase/liquibase \
                        --changeLogFile=changelog.xml \
                        --url=${DB_URL} \
                        --username=${DB_USER} \
                        --password=${DB_PASSWORD} \
                        update
                    """
                }
            }
        }
    }
}
