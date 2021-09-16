pipeline{
    agent any
    
    environment{
        PATH = "/usr/share/maven:$PATH"
    }
    stages{
        stage("Git Checkout"){
            steps{
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/Shubham1996-aws/tomcat.git'
            }
        }
        stage("Maven Build"){
            steps{
                sh "mvn clean package"
                sh "mv target/*.war target/myweb.war"
            }
        }
        stage("deploy-dev"){
            steps{
                sshagent(['4633f063-9f2e-4dd5-bf7a-632da1da296f']) {
                sh """
                    scp -o StrictHostKeyChecking=no target/myweb.war  ec2-user@172.31.84.34:/home/ec2-user/apache-tomcat-9.0.53/webapps/
                    
                    ssh ubuntu@172.31.84.34 /home/ec2-user/apache-tomcat-9.0.53/bin/shutdown.sh
                    
                    ssh ubuntu@172.31.84.34 /home/ec2-user/apache-tomcat-9.0.53/bin/startup.sh
                
                """
            }
            
            }
        }
    }
}
