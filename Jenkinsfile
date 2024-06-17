pipeline {
    agent any
    
    environment {
        DATABASE_URL = 'jdbc:mysql://54.224.127.177:3306/test_db'
        LIQUIBASE_HOME = '/usr/local/liquibase'
        LIQUIBASE_CLASSPATH = "${LIQUIBASE_HOME}/lib/mysql-connector-java-8.0.27.jar:${LIQUIBASE_HOME}/liquibase.jar"
    }
    
    stages {
        stage('Checkout') {
            steps {
                git credentialsId: 'github-token', url: 'https://github.com/Bairavir/your-repository.git', branch: 'main'
            }
        }
        stage('Apply Database Changes') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'test_db', passwordVariable: 'DB_PASSWORD', usernameVariable: 'DB_USERNAME')]) {
                    script {
                        echo 'Deploying SQL...'
                        sh '''
                            echo "Database URL: ${DATABASE_URL}"
                            echo "Database Username: ${DB_USERNAME}"
                            echo "Running Liquibase Update..."
                            export LIQUIBASE_CLASSPATH=${LIQUIBASE_HOME}/lib/*:${LIQUIBASE_HOME}/liquibase.jar
                            java -cp ${LIQUIBASE_CLASSPATH} liquibase.integration.commandline.Main \
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
