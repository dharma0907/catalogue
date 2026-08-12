pipeline {
    agent { 
        node { 
            label 'ROBOSHOP' 
        } 
    }
    environment {
        def appVersion = ""
        ACC_ID = "565139240657"
        project = "roboshop"
        component = "catalouge"
    }
    options {
        disableConcurrentBuilds()
        timeout(time: 15, unit: 'MINUTES')
    }
    // parameters {
    //     string(name: 'PERSON', defaultValue: 'Mr Jenkins', description: 'Who should I say hello to?')
    //     text(name: 'BIOGRAPHY', defaultValue: '', description: 'Enter some information about the person')
    //     booleanParam(name: 'DEPLOY', defaultValue: true, description: 'Toggle this value')
    //     choice(name: 'CHOICE', choices: ['One', 'Two', 'Three'], description: 'Pick something')
    //     password(name: 'PASSWORD', defaultValue: 'SECRET', description: 'Enter a password')
    // }
    // Build
    stages {
        stage('Read Version') {
            steps {
                     script {
                    def packageJson = readJSON file: 'package.json'
                    // Extract the version property
                    appVersion = packageJson.version
                    echo "The application version is: ${appVersion}"
                }
            }
        }
        stage('Install Dependencies') {
            steps {
                script {
                    sh """
                        npm install

                    """
                }
            }
        }
        // stage('Docker build') {
        //     steps {
        //         script {
        //             sh """
        //                 docker build -t catalogue:${appVersion} .

        //             """
        //         }
        //     }
        // }
        stage('Docker BUild and Push to ECR') {
            steps {
                
                script {
                // 'aws-global-creds' is the ID of your Jenkins credential entry
                        withAWS(credentials: 'aws-creds', region: 'us-east-1') {
                            sh """
                            aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin ${ACC_ID}.dkr.ecr.us-east-1.amazonaws.com
                            docker build -t ${project}/${component}:${appVersion} .
                            docker tag ${project}/${component}:${appVersion} ${ACC_ID}.dkr.ecr.us-east-1.amazonaws.com/${project}/${component}:${appVersion}
                            docker push ${ACC_ID}.dkr.ecr.us-east-1.amazonaws.com/${project}/${component}:${appVersion}
                            """
                        }
                }        
            }
        }

        stage('Deploy') {
            when {
                // Evaluates the boolean parameter directly
                expression { "${params.DEPLOY}" == "true" }
            }
            /* input {
                message "Should we continue?"
                ok "Yes, we should."
                submitter "alice,bob"
                parameters {
                    string(name: 'PERSON', defaultValue: 'Mr Jenkins', description: 'Who should I say hello to?')
                }
            } */
            steps {
                script {
                    sh """
                        echo "Deploying"
                    """
                }
            }
        }
    }

    post { 
        always { 
            echo 'I will always say Hello again!'
        }
        success { 
            echo 'I will run when success'
        }
        failure { 
            echo 'I will Run when it is failed'
        }
    }
}