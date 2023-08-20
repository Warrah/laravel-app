pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'composer install --no-interaction --no-dev --prefer-dist'
            }
        }

        stage('Run Database Migrations') {
            steps {
                sh 'php artisan migrate --force'
            }
        }

        stage('Deploy to Nginx') {
            steps {
                script {
                    echo 'Copying files to Nginx document root...'
                    sh 'sudo cp -r * /var/www/html/laravel/public'
                }

                // Reload Nginx to apply changes
                sh 'sudo systemctl reload nginx'
            }
        }
    }

    post {
        success {
            echo 'Deployment successful!'
        }
        failure {
            echo 'Deployment failed!'
        }
    }
}
