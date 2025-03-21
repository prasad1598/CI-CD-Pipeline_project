pipeline {
    agent any

    environment {
        SONAR_URL = 'http://your-sonarqube-server:9000'
        SONAR_PROJECT = 'your_project'
        NEXUS_URL = 'http://your-nexus-server:8081'
        NEXUS_REPO = 'maven-releases'
        DEPLOY_SERVER = 'user@your-server'
        DEPLOY_DIR = '/var/www/app'
    }

    stages {
        stage('Checkout Code') {
            steps {
                git 'https://github.com/example/repo.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Run Unit Tests') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Code Quality Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh """
                    mvn sonar:sonar \
                        -Dsonar.projectKey=${SONAR_PROJECT} \
                        -Dsonar.host.url=${SONAR_URL} \
                        -Dsonar.login=your_sonar_token
                    """
                }
            }
        }

        stage('Upload to Nexus') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'nexus-credentials', usernameVariable: 'NEXUS_USER', passwordVariable: 'NEXUS_PASS')]) {
                    sh """
                    mvn deploy \
                        -DaltDeploymentRepository=nexus::default::${NEXUS_URL}/repository/${NEXUS_REPO}/ \
                        -Dnexus.username=${NEXUS_USER} \
                        -Dnexus.password=${NEXUS_PASS}
                    """
                }
            }
        }

        stage('Deploy to Server') {
            steps {
                withCredentials([sshUserPrivateKey(credentialsId: 'deploy-ssh', keyFileVariable: 'SSH_KEY')]) {
                    sh """
                    scp -i $SSH_KEY target/*.jar ${DEPLOY_SERVER}:${DEPLOY_DIR}
                    ssh -i $SSH_KEY ${DEPLOY_SERVER} "systemctl restart myapp"
                    """
                }
            }
        }
    }
}
#dummy pipeline 