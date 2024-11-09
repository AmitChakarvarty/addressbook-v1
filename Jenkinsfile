pipeline {
    agent any

    parameters{
        string(name:'Env',defaultValue:'Test',description:'version to deploy')
        booleanParam(name:'executeTest',defaultValue: true,description:'decide to run tc')
        choice(name:'APPVERSION',choices:['1.1','1.2','1.3'])
    }

    stages {
        stage('Compile') {
            steps {
                echo 'Compiling the code'
                echo "compiling in env: ${params.Env}"
                sh "mvn compile"
            }
        }
        stage('CodeReview') {
            steps {
                echo 'Reviewing the Code'
                echo "Deploying the app version ${params.APPVERSION}"
                sh "mvn pmd:pmd"
            }
        }
        stage('UnitTest') {
            when{
                expression{
                    params.executeTest == true
                }
            }
            steps {
                echo 'UnitTest the Code'
                sh "mvn test"
            }
            post {
                always{
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('Package') {
            steps {
                echo 'Packaging the Code'
                echo "Deploying the app version ${params.APPVERSION}"
                sh "mvn package"
            }
        }
        stage('Deploy') {
            input{
                message "Select the platform to deploy"
                ok "Platform selected"
                parameters{
                    choice(name:'platform',choices:['ON-prem','EKS','EC2'])
                }
            }
            steps {
                echo 'Deploy the Code'
                echo "Deploying the app version ${params.APPVERSION}"
                echo "Depolying on ${params.platform}"
            }
        }
    }
}
