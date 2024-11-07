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
            }
        }
        stage('Package') {
            steps {
                echo 'Packaging the Code'
                echo "Deploying the app version ${params.APPVERSION}"
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
