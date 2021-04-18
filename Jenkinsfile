pipeline {
  agent {
    node {
      label 'agent1'
    }

  }
  stages {
    stage('Clone') {
      steps {
        git(url: 'https://github.com/elmehdisaniss/devops-webapp2', branch: 'master')
      }
    }

    stage('Build') {
      parallel {
        stage('Build') {
          steps {
            sh '''RELEASE=webapp.war
pwd
./gradlew build -PwarName=$RELEASE --info
ls -la build/libs/
cp ./build/libs/$RELEASE ./docker'''
          }
        }

        stage('P1') {
          steps {
            sh '''date
echo run parallel!!
'''
          }
        }

        stage('P2') {
          steps {
            sh '''date

echo run parallel!!
'''
          }
        }

      }
    }

    stage('Packaging') {
      steps {
        sh '''pwd
cd ./docker
docker build -t sanissdockerhubrepo/webapp1-2021:$BUILD_ID .
docker tag sanissdockerhubrepo/webapp1-2021:$BUILD_ID sanissdockerhubrepo/webapp1-2021:latest
docker images'''
      }
    }

    stage('Publish') {
      steps {
        script {
          withCredentials([usernamePassword(credentialsId: 'ca-dockerhub', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]){
            sh '''
docker login -u="$DOCKER_USERNAME" -p="$DOCKER_PASSWORD"
docker push sanissdockerhubrepo/webapp1-2021:$BUILD_ID
docker push sanissdockerhubrepo/webapp1-2021:latest
'''
          }
        }

      }
    }

  }
}