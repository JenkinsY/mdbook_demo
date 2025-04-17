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
                sh 'apt-get update && apt-get install -y build-essential'
                sh 'cargo install mdbook || echo "mdBook already installed"'
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
                    mkdir -p ${DEPLOY_DIR}
                    rm -rf ${DEPLOY_DIR}/*
                    cp -r ${MDBOOK_BUILD_DIR}/* ${DEPLOY_DIR}/
                '''
            }
        }
    }

    post {
        success {
            echo '✅ mdBook 构建并部署成功！'
        }
        failure {
            echo '❌ 构建失败。'
        }
    }
}