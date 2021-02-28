pipeline {
    agent any
    environment {
        dockerRun = "docker run -p 8000:80 -d --name myclouddemo  ashishut/demo-project:latest"
        dockerrm = "docker container rm -f myclouddemo"
        dockerimagerm = "docker image rmi  ashishut/demo-project"
    }
        

    stages {
        stage('PUll') {
            steps {
                git 'https://github.com/Ashish24Dez/kubernetesproject.git'
            }
        }
        
        stage("Build") {
            steps {
                sh 'docker image build -t $JOB_NAME:v1.$BUILD_ID .'
                sh 'docker image tag $JOB_NAME:v1.$BUILD_ID ashishut/$JOB_NAME:v1.$BUILD_ID'
                sh 'docker image tag $JOB_NAME:v1.$BUILD_ID ashishut/$JOB_NAME:latest'
                
            }
        }
        
        stage("Push") {
            steps {
                withCredentials([string(credentialsId: 'mobilehubpassword', variable: 'mobilehubpassword')]) {
    // some block
    sh 'docker login -u ashishut -p ${mobilehubpassword}'
    sh 'docker image push ashishut/$JOB_NAME:v1.$BUILD_ID'
    sh 'docker image push ashishut/$JOB_NAME:latest'
    sh 'docker image rmi $JOB_NAME:v1.$BUILD_ID ashishut/$JOB_NAME:v1.$BUILD_ID ashishut/$JOB_NAME:latest'
}
            }
        }
        
        stage("Deployment") {
            steps {
             sshagent(['hostpassword']) {
    // some block           
   
                 
                 sh "ssh -o StricHostKeyChecking=no ec2-user@172.31.29.113  ${env.dockerRun}"
    
}
        }
        }
    }
}
