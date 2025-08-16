pipeline {
    agent any

    stages {

        // Git pull on worker
        stage('Git Pull with Checkout Branch') {
            steps {
                echo "git pull..."
                echo "END stage"
            }
        }

        // Revert VM Snapshot
        stage('Revert Snapshot of ICM machines.. with Validating') {
            steps {
                script {
                    echo 'Reverting changes and validating..'
                }
            }
        }

        // Install ES on VM
        stage('Install ES on VM with Validating') {
            steps {
                script {
                    echo 'ES Install...!'
                }
            }
        }

        // Automation Suite Triggering
        stage('Automation Suite Triggering') {
            stages {
                
                // Common Ground Suite
                stage('Common Ground') {
                    steps {
                        echo 'Running Common Ground automation suite..'
                    }
                }

                // Parallel execution: Admin Client + ISE Client
                stage('Admin Client & ISE Client Parallel') {
                    parallel failFast: true, stages: [
                        'Admin Client Install': {
                            stage('Admin Client Install') {
                                steps {
                                    echo 'Installing Admin Client...'
                                }
                            }
                        },
                        'ISE Client Install': {
                            stage('ISE Client Install') {
                                steps {
                                    echo 'Installing ISE Client...'
                                }
                            }
                        }
                    ]
                }

                // ICM Uninstall Automation
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
            echo 'Pipeline failed! Please check logs.'
        }
    }
}
