pipeline {
    agent any

    tools {
        // Assuming 'jfrog-cli' is correctly configured in your Jenkins instance
        jfrog 'jfrog-cli'
    }

    stages {
        stage('Maven build artifact') {
            steps {
                sh 'mvn clean package -DskipTests=true'
            }
        }

        stage('Building a Docker image') {
            steps {
                script {
                    dockerImage = docker.build("siddeo10/petapp:${BUILD_NUMBER}", ".")
                }
            }
        }

        stage('Docker image push') {
            steps {
                script {
                    docker.withRegistry('', 'Docker-hub') {
                        dockerImage.push()
                    }
                }
            }
        }

        stage('Push artifact') {
            steps {
                script {
                    sh 'jfrog rt u /var/lib/jenkins/workspace/Petclinic-jar/target/spring-petclinic-3.2.0-SNAPSHOT.jar petclinic_artifact-libs-snapshot-local/springpetclinic/spring-petclinic-3.2.0-SNAPSHOT.jar'
                }
            }
        }
    }
}
