pipeline{
    agent any
    tools{
        maven 'M3'
    }
    environment{
        JAVA_HOME='/usr/lib/jvm/java-11-openjdk-11.0.14.1.1-1.el7_9.x86_64'
    }
    stages{
        stage('cloing the code'){
            steps{
                git branch: 'main', changelog: false, poll: false, url: 'https://github.com/abnavemangesh22/myboschrepo.git'
            }
        }

        stage('code compile'){
            steps{
                sh 'mvn compile'
            }
        }
        
        stage('sonarqube'){
            steps{
                script{
                    withSonarQubeEnv(credentialsId: 'sonar') {
                        sh 'mvn sonar:sonar'
                      }
                }
            }
        }
        
        stage ('approval'){
            steps{
                script{
                    timeout(10) {
                        input(id: "Deploy Gate", message: "Deploy ${params.project_name}?", ok: 'Deploy')
                    }
                }
            }
        }
        
        stage('Package'){
            steps{
                sh 'mvn package'
            }
        }
      
    }
  }

