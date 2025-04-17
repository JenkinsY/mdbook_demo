pipeline {
    agent {
        docker {
            image 'rust:latest'
            args '-v /cargo-cache:/usr/local/cargo/registry'
        }
    }

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

        stage('Install mdBook') {
            steps {
                sh 'cargo install mdbook || echo "mdBook already installed"'
            }
        }

        stage('Build Book') {
            steps {
                sh 'mdbook build'
            }
        }

        stage('Deploy to Nginx') {
            agent { label 'jenkins' }  // 可自定义执行节点
            steps {
                sh '''
                echo "Deploying to Nginx..."
                sudo rm -rf ${DEPLOY_DIR}/*
                sudo cp -r ${MDBOOK_BUILD_DIR}/* ${DEPLOY_DIR}/
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