pipeline {
    agent any

    environment {
        // Defining environment variables
        MY_VAR = 'Hello Jenkins'
        BUILD_DATE = "${new Date().format('yyyy-MM-dd HH:mm:ss')}"
    }

    stages {
        stage('Preparation') {
            steps {
                script {
                    echo "Starting build process"
                    echo "Build initiated at: ${env.BUILD_DATE}"
                }
            }
        }

        stage('Build') {
            steps {
                echo "Building the project..."
                // Add build commands here, e.g.:
                // sh 'mvn clean install'
            }
        }

        stage('Test') {
            steps {
                echo "Running tests..."
                // Add testing commands here, e.g.:
                // sh 'mvn test'
            }
        }

        stage('Deploy') {
            steps {
                echo "Deploying the application..."
                // Add deployment commands here, e.g.:
                // sh './deploy.sh'
            }
        }

        stage('Completion') {
            steps {
                echo "Build completed successfully"
            }
        }
    }

    post {
        always {
            echo "This will run regardless of success or failure."
        }
        success {
            echo "The pipeline ran successfully."
        }
        failure {
            echo "The pipeline failed."
        }
    }
}
