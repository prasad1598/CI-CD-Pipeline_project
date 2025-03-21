pipeline {
    agent {label 'Slave_Node'}
    stages {
        stage ('Clone project') {
            steps {
                echo "Repository cloning"
                git 'https://github.com/prasad1598/java-maven-project-new.git'

            }
        }
        stage ('Compile code') {
            steps {
                echo "compiling the source code"
                sh 'mvn compile'
            }
        }
        stage ('Test') {
            steps {
                echo "Test the sourcecode"
                sh 'mvn test'
            }
        }
        stage ('Genarate artifact') {
            steps {
                echo "Genarating Artifact files"
                sh 'mvn clean package'
            }
        }
        stage ('Deployment') {
            steps {
                echo "Deploy in tomcat"
                deploy adapters: [tomcat9(credentialsId: 'Tomcat', path: '', url: 'http://3.86.109.226:8080/')], contextPath: 'Myapplication', war: '**/*.war'
            }
        }
    }
}