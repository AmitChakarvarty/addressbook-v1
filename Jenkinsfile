pipeline {

    agent any


    parameters {

        string(name: 'ENVIRONMENT', defaultValue: 'dev', description: 'Environment to deploy to')

        booleanParam(name: 'RUN_TESTS', defaultValue: true, description: 'Run tests?')

        choice(name: 'DEPLOY_SERVER', choices: ['dev', 'test', 'prod'], description: 'Choose deployment server')

    }


    stages {

        stage('Dev Stage') {

            steps {

                script {

                    if (params.ENVIRONMENT == 'dev') {

                        echo "Building for development environment"

                    }

                }

            }

        }

        stage('Test Stage') {

            steps {

                script {

                    if (params.RUN_TESTS) {

                        echo "Running tests in the ${params.ENVIRONMENT} environment"

                    } else {

                        echo "Skipping tests"

                    }

                }

            }

        }

        stage('Prod Stage') {

            steps {

                script {

                    if (params.DEPLOY_SERVER == 'prod') {

                        echo "Deploying to production server"

                    } else {

                        echo "Deploying to ${params.DEPLOY_SERVER} server"

                    }

                }

            }

        }

    }

}