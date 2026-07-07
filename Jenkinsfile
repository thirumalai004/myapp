pipeline{
    agent any
    tools{
        maven 'new-mvn'
    }
    environment{
        DOCKER_IMAGE='my-html'
        CONTAINER_NAME='my-html-container-2' 
    }
    parameters{
        choice(name: 'ENVIRONMENT' , choices: ['testing' , 'prod'], description: 'environment')
        
    }
    stages{
        stage('git clone'){
            steps{
                git branch: 'main', credentialsId: 'myapp-credential', url: 'https://github.com/thirumalai004/myapp.git'
            }
        }
        stage('maven test stage'){
            when{
                expression{
                     params.ENVIRONMENT =='testing'
                }
            }
            steps{
                bat 'mvn test'
            }
        }   
        stage('maven build stage'){
            steps{
                bat 'mvn clean install -DskipTests=true'
            }
        }   
        stage('docker image build stage'){
            steps{
                bat ''' set "PATH=C:\\Program Files\\Docker\\Docker\\resources\\bin;%PATH%"
                        docker build -t %DOCKER_IMAGE%:%BUILD_NUMBER% .
                        '''
            }
        } 
        stage('docker old rm  stage'){
            steps{
                bat ''' set "PATH=C:\\Program Files\\Docker\\Docker\\resources\\bin;%PATH%"
                        docker rm -f  %CONTAINER_NAME%
                        '''
            }
        } 
         stage('docker new run  stage'){
            steps{
                bat ''' set "PATH=C:\\Program Files\\Docker\\Docker\\resources\\bin;%PATH%"
                        docker run -d -p 8081:80 --name  %CONTAINER_NAME%  %DOCKER_IMAGE%:%BUILD_NUMBER%
                        '''
            }
        } 


            }
        }    
        

