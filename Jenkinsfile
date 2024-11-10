pipeline {
    agent none
    tools{
        maven 'mymaven'
    }

    parameters{
        string(name:'Env',defaultValue:'Test',description:'version to deploy')
        booleanParam(name:'executeTest',defaultValue: true,description:'decide to run tc')
        choice(name:'APPVERSION',choices:['1.1','1.2','1.3'])
    }
    environment {
        DEV_SERVER='ec2-user@172.31.0.253'
    }
    stages {
        stage('Compile') {
            agent any
            steps {
                echo 'Compiling the code'
                echo "compiling in env: ${params.Env}"
                sh "mvn compile"
            }
        }
        stage('CodeReview') {
            agent any
            steps {
                echo 'Reviewing the Code'
                echo "Deploying the app version ${params.APPVERSION}"
                sh "mvn pmd:pmd"
            }
          //  post {
          //      always {
          //          pmd pattern: 'target/pmd.xml'
          //      }
          //  }
        }
        stage('UnitTest') {
            agent {label 'linux_slave'}
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
            //agent {label 'linux_slave'}
            agent any
            steps {
                script{
                sshagent(['slave2']) {
                echo 'Packaging the Code'
                echo "Deploying the app version ${params.APPVERSION}"
                sh "scp -o StrictHostKeyChecking=no server-script.sh ${DEV_SERVER}:/home/ec2-user"
                sh "ssh -o StrictHostKeyChecking=no ${DEV_SERVER} 'bash /home/ec2-user/server-script.sh'"
            }
        }
            }
         }
        stage('Deploy') {
            agent any
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
