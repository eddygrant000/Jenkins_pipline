pipeline{
    agent any
    environment{
        PATH="/usr/bin/mvn:$PATH"
    }
    stages{
        stage('Source Code CheckOut'){
            steps{
                git url: 'https://github.com/daticahealth/java-tomcat-maven-example'
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
        stage("Quality gate") {
            steps {
                waitForQualityGate abortPipeline: true
            }
        }
        stage('Fetch_Infra_Code') {
        steps {
          git branch: 'main', url: 'https://github.com/eddygrant000/Terraform_Ansible.git'
        }
      }

      stage('TF Init&wPlan') {
        steps {
          sh 'terraform init'
          sh 'terraform validate'
          sh 'terraform plan -var-file=secret.tfvars'
        }      
      }
      stage('TF Apply') {
        steps {
          sh 'terraform apply -var-file=secret.tfvars --auto-approve'
        }
      }
      stage('Deploy Application'){
          steps{
              sh 'ansible-playbook play1.yml'
              sh 'ansible-playbook play2.yml -e jobname=$JOB_NAME'
          }
      }
    }
}
