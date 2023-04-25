pipeline {
    agent any
    parameters {
        string(name: 'WEB_APP', description: 'WEBAPP image tag seven first characters')
        booleanParam(name: 'DEV', defaultValue: false, description: 'Deploy to DEV?')
        booleanParam(name: 'PROD', defaultValue: false, description: 'Deploy to PROD?')
    }

    environment {
        PROJECT_NAME = "magazine-spa"
    }

    options {
        disableConcurrentBuilds()
        skipDefaultCheckout()
    }

    stages {
        stage("Preparación") { 
            steps {
                script {
                    if (params.DEV) {
                        sh "docker-compose -f deployment/dev.yml down"
                    }
                    if (params.PROD) {
                        sh "docker-compose -f deployment/prod.yml down"
                    }
                }
            }
        }
        stage("Deploy to DEV") {
            when {
                allOf {
                    environment name: 'DEV', value: 'true'
                }
            }
            steps {
                sh "export WEB_APP=${WEB_APP}"
                sh "docker-compose -f deployment/dev.yml up -d"
            }
        }
        stage("Deploy to PROD") {
            when {
                allOf {
                    environment name: 'PROD', value: 'true'
                }
            }
            steps {
                sh "export WEB_APP=${WEB_APP}"
                sh "docker-compose -f deployment/prod.yml up -d"
            }
        }
    }
}
