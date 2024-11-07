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
                echo "compiling in env: ${params.ENV}"
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
    }
}
