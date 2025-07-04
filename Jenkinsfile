pipeline {
    agent any 

    parameters {
        choice(name: 'DEPLOY_ENV', choices: ['blue', 'green'], description: 'Choose which environment to deploy: Blue or Green')
        choice(name: 'DOCKER_TAG', choices: ['blue', 'green'], description: 'Choose the Docker image tag for the deployment')
        booleanParam(name: 'SWITCH_TRAFFIC', defaultValue: false, description: 'Switch traffic between Blue and Green')
    }
    
    environment {
        IMAGE_NAME = "vsiraparapu/bankapp"
        TAG = "${params.DOCKER_TAG}"
        KUBE_NAMESPACE = 'webapps'
        // SCANNER_HOME = tool 'sonar-scanner'
    }

    stages {
        stage('mvn install'){
            steps{
                 sh "apt install openjdk-17-jdk -y"
                 sh "apt install maven -y"
            }
        }

        stage('Permissions') {
            steps {
                sh "chmod -R 777 mvnw"
            }
        }
       
        stage('Compile') {
            steps {
                sh "mvn compile"
            }
        }
        
        stage('Tests') {
            steps {
                sh "mvn clean test -X -DskipTests=true"
            }
        }
        
        stage('Build') {
            steps {
                sh "mvn package -DskipTests=true"
            }
        }
    }
}

