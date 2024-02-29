pipeline {
    agent any

    stages {
        stage('checkout') {
            stages { 
                /*
                stage('pwd') {
                    steps {
                        sh'''ls'''
                    }
                }
                */
                stage('clear workspaces') {
                    steps {
                        sh'''
                        rm -r /var/lib/jenkins/workspace/testCollection/* 
                        '''
                    }
                }   
            }
        }    
        stage('clone repo') {
            steps {
                //withCredentials(usernamePassword(credentialsId :jenkins-user-github ,passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME' )){
                // Get some code from a GitHub repository
                sh'''
                git clone https://github.com/Belamstel/TestCollection.git

                echo "pulled the code"
                '''
            }
        }
        stage('Test 1') {
            steps {
                catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                    sh'''
                    cd TestCollection
                    newman run TestCollection.postman_collection.json -r allure --reporter-allure-export test-results
                    '''
                }    
            }
        }
        stage('Test 2') {
            steps {
                sh'''
                cd TestCollection
                newman run TestCollection2.postman_collection.json -r allure --reporter-allure-export test2-results
                '''
            }
        }
        /*
        stage('ls') {
            steps {
                sh'''
                cd TestCollection
                ls
                '''
            }
        }
        */
        stage('gen') {
            steps {
                allure includeProperties: false, jdk: '', results: [[path: 'TestCollection/test-results'], [path: 'TestCollection/test2-results']]
            }
        }
    }
}
