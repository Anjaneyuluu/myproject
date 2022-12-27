pipeline {
    agent any
    stages {
        
        stage('Maven Build package'){
            steps {
                sh " mvn clean package"
            }
        }
        stage('Docker Build '){
            steps{
                sh " docker build -t mrofficialnah/myproject:0.0.2 ."
                }
            }
        stage ('Docker push') {
            steps {
               withCredentials([string(credentialsId: 'Docker-Hub', variable: 'hubPwd')] {
                   sh "docker login -u mrofficialnah -p ${hubPwd}"
                   sh "docker push mrofficialnah/myproject:0.0.2"
              }    
            }
        } 
        stage('Docker Deploy') {
            steps {
                sshagent(['docker-host']) {
                    sh "ssh -o StrictHostKeyChecking=no ec2-user@172.31.33.135 docker run -d -p 8080:8080 --name myproject mrofficialnah/myproject:0.0.2"
                }
            }
            
        }
        
    }
}
