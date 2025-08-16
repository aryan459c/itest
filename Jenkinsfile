pipeline {
    agent any

    stages {

        stage('Git Pull with Checkout Branch') {
            steps {
                echo "git pull..."
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
                    parallel {
                        failFast true

                        stage('Admin Client Install') {
                            steps {
                                echo 'Installing Admin Client...'
                                // error("For testing failFast: failing Admin") 
                            }
                        }

                        stage('ISE Client Install') {
                            steps {
                                echo 'Installing ISE Client...'
                            }
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
            echo '✅ Pipeline completed successfully!'
        }
        failure {
            echo '❌ Pipeline failed! Please check logs.'
        }
    }
}
