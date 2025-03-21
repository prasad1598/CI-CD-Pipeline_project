pipeline {
    agent {label 'Slave_Node'}
    stages {
        stage ('Clone project') {
            steps {
                echo "Repository cloning"
                git 'https://github.com/prasad1598/java-maven-project-new.git'

            }
        }
    }
}