pipeline {
  agent any
  stages {
    stage ('Initial Cleanup') {
      steps {
        echo 'This is Initial Cleanup'
        deleteDir()
      }
    }
    stage ('Code Pull') {
      steps {
        echo 'This is the second stage to clone the code from GITHUB'
        git $GIT_REPO_PATH
      }
    }
    stage ('DOCKER IMAGE BUILD') {
      steps {
        echo 'This is the 3rd stage where Docker image build using //Docker file from the repo'
        sh 'git build -t $GIT_IMAGE_NAME .'
      }
    }
    steps {
        withCredentials([usernamePassword(
            credentialsId: 'dockerhub-creds',
            usernameVariable: 'DOCKER_USER',
            passwordVariable: 'DOCKER_PASS'
        )]) {
            sh '''
                echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
            '''
        }
    }
    stage ('DOCKER IMAGE DEPLOY') {
      steps {
        echo 'This is the 4th stage where Docker image creation'
        sh 'git run -itd --name $CONT_NAME -p $PORT_NUM:80 $GIT_IMAGE_NAME'
      }
    }
  }
}
