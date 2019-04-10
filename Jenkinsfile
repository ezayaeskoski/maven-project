pipeline{
    agent any

parameters{
    string(name:'tomcat_dev',defaultValue:'35.160.236.71',description:'Staging Server')
    string(name:'tomcat_prod',defaultValue:'52.37.158.33',description:'Production Server')
}

triggers{
    pollSCM('* * * * *')
}

stages{
        stage('Build'){
            steps {
                sh 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }

        stage ('Deployments'){
            parallel{
                stage ('Deploy to Staging'){
                    steps {
                        sh "scp -i /home/ilegra/Documentos/projetos/estudos/amazon-ec2/tomcat-demo3.pem **/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat7/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        sh "scp -i /home/ilegra/Documentos/projetos/estudos/amazon-ec2/tomcat-demo3.pem **/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat7/webapps"
                    }
                }
            }
        }
    }
  
}