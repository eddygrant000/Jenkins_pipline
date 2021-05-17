pipeline{
    agent any
    environment{
        PATH="/usr/bin/mvn:$PATH"
    }
    stages{
        stage('SCM CheckOut'){
            steps{
                git url: 'https://github.com/daticahealth/java-tomcat-maven-example'
            }
        }

        stage("Build"){
            steps{
                sh 'mvn clean install'
            }
        }
        stage('Compile'){
            steps{
                sh 'mvn compile'
            }
        }
        stage('Junit Test'){
            steps{
                sh 'mvn test'
            }
        }
        stage('Sonarqube'){
            steps{
                withSonarQubeEnv(credentialsId: 'sachin', installationName: 'sonar') // remaining
                {
                    sh 'mvn sonar:sonar'
                }
            }
        }
        stage("Quality gate") {
            steps {
                waitForQualityGate abortPipeline: true
            }
        }
    }
}
