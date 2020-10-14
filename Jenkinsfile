pipeline {
  agent {

    docker {
      image 'nexus.agwes.net:8443/buildcontainer'
      label 'jenkins-node'
    }

  }
    stages {
        stage ('git') {
            steps {
                git 'https://github.com/ag4544/boxfuse-sample.git'
            }
        }
        stage ('build') {
            steps {
                sh 'mvn package'
            }
        }
        stage ('create docker image') {
            steps {
              sh 'cp -R target/hello-1.0.war docker/ROOT.war && cd docker/ && docker build --tag=boxfuse-prod .'
              sh 'docker login nexus.agwes.net:8443 -u admin -p HRtlop34'
              sh '''docker tag boxfuse-prod nexus.agwes.net:8443/boxfuse-prod && docker push nexus.agwes.net:8443/boxfuse-prod'''
            }
        }
        stage ('pull docker image to webserver') {
            agent {
              label 'jenkins-node'
            }
            steps {
              sh 'ssh root@10.130.0.20 'docker login nexus.agwes.net:8443 -u admin -p HRtlop34''
              sh 'ssh root@10.130.0.20 'docker run nexus.agwes.net:8443/boxfuse-prod''
           }
    }
}
}
