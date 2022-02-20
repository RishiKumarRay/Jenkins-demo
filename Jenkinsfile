@Library('my-maven-library')_
pipeline{

    agent {
        label 'slave'
    }

     tools {
         jdk 'jdk'
         maven 'Maven' 
    }  
    environment {
        dockerhub=credentials('dockerhub')
        
    } 
    stages{
        stage('Build')
        {   
            steps {
         mavenBuild()
          }
        }
       
        stage('test')
        {
            steps
                {
                runTests()
            }
        }   
  
        stage('pack')
        {
            when{
                branch "Production"
                }
            steps{
               mavenPackage()
            }
        }
        stage('build image')
        {
            when{
                branch "Production"
                }
            steps{
                dockerBuild( myimage:"${GIT_COMMIT}")
            }
        } 

        stage('pushing to dockerhub')
        {
            when{
                branch "Production"
                }
            steps{
                sh 'docker tag myimage:${GIT_COMMIT} rishiray/hello-world:${GIT_COMMIT}'
                sh 'echo $dockerhub_PSW | docker login -u $dockerhub_USR --password-stdin'
                sh 'docker push rishiray/hello-world:${GIT_COMMIT}'
            }
        } 

    }
}
