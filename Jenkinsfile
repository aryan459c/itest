pipeline {
    agent any

    stages {

        stage('Git Pull with Checkout Branch') {
            steps {
                echo "git pull..."
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'ti', url: 'git@github.com:aryan459c/itest.git']])
                echo "END stage"
            }
        }

        stage('Revert Snapshot of ICM machines.. with Validating') {
            steps {
                echo 'Reverting changes and validating..'
            }
        }

        stage('Install ES on VM with Validating') {
            steps {
                echo 'ES Install...!'
            }
        }

        stage('Automation Suite Triggering') {
            stages {

                stage('Common Ground') {
                    steps {
                        echo 'Running Common Ground automation suite..'
                    }
                }

                stage('Admin Client & ISE Client Parallel') {
                    steps {
                        script {
                            parallel (
                                failFast: true,
                                "Admin Client Install": {
                                    echo 'Installing Admin Client...'
                                    // error("Failing Admin for test") // uncomment to test failFast
                                },
                                "ISE Client Install": {
                                    echo 'Installing ISE Client...'
                                }
                            )
                        }
                    }
                }

                stage('ICM Uninstall Automation') {
                    steps {
                        echo 'Running ICM uninstall automation...'
                    }
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo ' Pipeline failed! Please check logs.'
        }
    }
}
