def initializeEnvironment(){
    echo "initializeEnvironment"
    def constantJson

    if(isUnix()){
       constantJson = readJSON file: "${WORKSPACE}/TestAutomation/Framework/JenkinsScripts/Constants.json"
    } else {
        constantJson = readJSON file: "${WORKSPACE}\\TestAutomation\\Framework\\JenkinsScripts\\Constants.json"
    }

    env.RoomId                   =   "${constantJson.ROOM_ID.CCE}"
    env.TestManagerServer        =   "${constantJson.TestManagerServer}"
    env.CCE_RELEASE_NUMBER       =   "R_15.0.2"
    env.JENKINS_BUILD_NUMBER     =   "RUN_${BUILD_NUMBER}"
    env.UTILS_SCRIPT             =   "${WORKSPACE}${constantJson.UTILS_SCRIPT}"
    env.AUTOMATION_SCRIPT        =   "${WORKSPACE}${constantJson.AUTOMATION_SCRIPT}"
    env.UCCE_AUTO_HOME_DIR       =   "${constantJson.UCCE_AUTO_HOME_DIR}"
}


pipeline{
    stages{

      //GIT pull on worker
        stage('Git Pull with Checkout Branch'){
            steps {
                timestamps {
                   echo "START stage:${env.STAGE_NAME}"
                    script {
                        // Retrieve the list of labels from the properties
                        def nodeList = ListofNodes.split(',')
    
                        parallel nodeList.collectEntries {
                            label ->
                            ["Executing on ${label}" : {
                                node(label) {
                                    // Below Code to be executed on each label
                                        dir('c://ucce_auto'){
                                        checkout([$class: 'GitSCM',
                                        branches: [[name: "${BRANCH_NAME}"]],
                                        userRemoteConfigs: [[url: 'git@github4-chn.cisco.com:ccbu-ucce/ucce_auto.git']]])
									}
                                }
                            }]
                        }
                    }
                    echo "END stage:${env.STAGE_NAME}"
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
