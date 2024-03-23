pipeline {
    agent any
  
    tools {
        jfrog 'jfrog-cli'
    }

        stage('Maven build artifact') {
            steps {
                sh 'mvn clean package -DskipTests=true'
            }
        }


        stage('Building a Docker image') {
            steps {
                sh "docker build -t siddeo10/petapp:${BUILD_NUMBER} ."
            }
        }


        stage('Docker image push') {
            steps {
                withDockerRegistry(credentialsId: 'Docker-hub', url: '') {
                    sh "docker push siddeo10/petapp:${BUILD_NUMBER}"
                }
            }
        }


        stage('Push artifact') {
            steps {
                jf 'rt u /var/lib/jenkins/workspace/pipeline_part2/target/spring-petclinic-3.2.0-SNAPSHOT.jar petclinic_artifact-libs-snapshot-local/springpetclinic/spring-petclinic-3.2.0-SNAPSHOT.jar'
            }
        }
    }
}
