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
                script {
            // Set your database credentials here
            env.DB_HOST = 'localhost'
            env.DB_PORT = '3606'
            env.DB_DATABASE = 'laravel'
            env.DB_USERNAME = 'laraveluser'
            env.DB_PASSWORD = '123'
        }
                sh 'php artisan migrate --force'
            }
        }
        stage('Run Tests') {
            steps {
                sh 'php artisan test'
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
