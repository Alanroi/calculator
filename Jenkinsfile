pipeline {
    agent any
    parameters {
        choice(name: 'ENVIRONMENT',
            choices: ['QA' , 'PRODUCTION','TEST'],
            description: 'Choose the environment for this deployment')
    }

    stages {
        stage ('Deploy to QA environments') {
            when {
                expression { params.ENVIRONMENT == 'QA' }
            }
            steps {
                echo "Deploying to ${params.ENVIRONMENT}"
                bat "mvn clean install"
            }
        }
        stage ('Deploy to TEST environments') {
                    when {
                        expression { params.ENVIRONMENT == 'TEST' }
                    }
                    steps {
                        echo "Deploying to ${params.ENVIRONMENT}"
                        bat "mvn clean test"
                    }
                }
        stage ('Deploy to production environment') {
            when {
                expression { params.ENVIRONMENT == 'PRODUCTION' }
            }
            steps {
                input message: 'Confirm deployment to production...', ok: 'Deploy'
                echo "Deploying to ${params.ENVIRONMENT}"
                bat "mvn clean install"
            }
        }
    }
    post{
        always{
            archiveArtifacts allowEmptyArchive: true,
            artifacts: '**',
            fingerprint: true,
            followSymlinks: false,
            onlyIfSuccessful: true
        }
    }
}