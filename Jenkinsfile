
pipeline{
    stages{

      //GIT pull on worker
        stage('Git Pull with Checkout Branch'){
            steps {
                echo "gitpull "
                    }
                    echo "END stage"
                }
            }
        }
        //End of Git Pull

        //Reverting VM Snapshot With Validating
        stage('Revert Snapshot of ICM machines.. with Validating'){
            steps{
                script{
                    echo 'Reverting changes and validating..'
                    
                }
            }
        }
        //End of Reverting VM Snapshot With Validating

        //Install ES on VM
        stage('Install ES on VM with Validating'){
            steps{
                script{
                    echo 'ES Install...!'
                }
                
            }
        }
        //End Of ES Install

        //Atomation Suit Triggring
        stage('Automation Suite Triggering'){
            parallel failFast:true,        //Fail any parallel stage --> stop all
            stages:[
                //Start Common Ground Suite
                stage('Common Ground'){
                    steps{
                        echo 'Running Common Ground automation suite..'

                    }
                }
                //End Common Ground Suite

                //Parallel Execution (Admin Client + ISE Client)
                stage('Admin Client & ISE Client Parallel'){
                    parallel{
                        //Admin Client Suite
                        stage('Admin Client Install'){
                            steps{
                                echo 'Instaling Admin Clien...'
                            }
                        }
                        //End of Admin Client Suite

                        //ISE Client Suite
                        stage('ISE Client Install'){
                            steps{
                                echo 'Installing ISE Client . . .'

                            }
                        }
                        //End of ISE Client Suite
                    }
                    //End of parallel Execution
                }

                //ICM Uninstall Automation
                stage('ICM Uninstall Automation'){
                    steps{
                        echo 'Running ICM uninstall automation. . .'

                    }
                }
                //End of ICM Uninstall Automation
            ]
        }
    }
    post{
        success{
            echo 'Pipeline completed successfully!'
        }
        failure{
            echo 'Pipeline failed Please check logs'
        }
    }
}
