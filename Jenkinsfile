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
       stage('Test') {
            steps {
                echo 'Testing.....'
            }
        }

        stage('Deploy to Nginx') {
            steps {
                script {
                    echo 'Copying files to Nginx document root...'
                    sh 'sudo cp -r * /var/www/html/laravel/'
                }

                // Reload Nginx to apply changes
                sh 'sudo systemctl reload nginx'
            }
        }
    }

    post {
        success {
        echo 'Deployment successful!'
        slackSend(
            channel: '#jenkins_personal', // Specify the Slack channel or user to send notifications to
            color: 'good',
            message: "Deployment of your laravel project was successful! See localhost:8200"
        )
    }
    failure {
        echo 'Deployment failed!'
        slackSend(
            channel: '#jenkins_personal', // Specify the Slack channel or user to send notifications to
            color: 'danger',
            message: "Deployment of your laravel project failed."
        )
    }
    }
}
