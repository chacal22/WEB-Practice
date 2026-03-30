  pipeline {
      agent any

      environment {
          WEB_ROOT = '/home/mcherlo/docker/jenkins/web'
          CONTAINER_NAME = 'apache1'
      }

      stages {
          stage('Request deployment data') {
              input {
                  message 'Enter the data'
                  parameters {
                      string(name: 'AUTHOR', defaultValue: 'Sergio', description: 'Author of the web application deployment')
                      string(name: 'ENVIRONMENT', defaultValue: 'Development', description: 'Environment to deploy')
                  }
              }
              steps {
                  echo "The responsible of this project is ${AUTHOR} and will be deployed in ${ENVIRONMENT}"
              }
          }

          stage('Stop previous Apache container') {
              steps {
                  echo 'Dropping previous container if it exists...'
                  sh 'docker rm -f ${CONTAINER_NAME} || true'
              }
          }

          stage('Prepare web directory') {
              steps {
                  echo 'Preparing web root directory...'
                  sh 'mkdir -p ${WEB_ROOT}'
                  sh 'find ${WEB_ROOT} -mindepth 1 -maxdepth 1 -exec rm -rf {} +'
              }
          }

          stage('Copy web application') {
              steps {
                  echo 'Copying web application...'
                  sh 'cp -r web/. ${WEB_ROOT}/'
              }
          }

          stage('Create Apache httpd container') {
              steps {
                  echo 'Creating the container...'
                  sh 'docker run -dit --name ${CONTAINER_NAME} -p 9000:80 -v ${WEB_ROOT}:/usr/local/apache2/htdocs/ httpd'
              }
          }

          stage('Check application') {
              steps {
                  echo 'Testing the web app...'
                  sh 'wget -qO- --timeout=10 http://jenkins-master:9000 > /dev/null'
              }
          }
      }
  }
