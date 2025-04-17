pipeline {
    agent any

    environment {
        MDBOOK_BUILD_DIR = "book"
        DEPLOY_DIR = "/var/www/mdbook_site"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Book') {
            steps {
                sh 'mdbook build'
            }
        }

        stage('Deploy to Nginx') {
            steps {
                sh '''
                echo "Deploying to Nginx..."
                rm -rf ${DEPLOY_DIR}/*
                cp -r ${MDBOOK_BUILD_DIR}/* ${DEPLOY_DIR}/
                '''
            }
        }
    }

    post {
        success {
            echo '✅ mdBook build & deployed successfully!'
        }
        failure {
            echo '❌ Build failed.'
        }
    }
}