  pipeline {
      agent any

      environment {
          HOST_WEB_ROOT = '/home/mcherlo/docker/jenkins/tomcat-web'
          CONTAINER_NAME = 'tomcat1'
          APP_NAME = 'shopping'
      }

      stages {
          stage('Prepare directory for the web application') {
              steps {
                  echo 'Preparing deployment directory...'
                  sh 'mkdir -p ${HOST_WEB_ROOT}/${APP_NAME}'
                  sh 'find ${HOST_WEB_ROOT}/${APP_NAME} -mindepth 1 -maxdepth 1 -exec rm -rf {} +'
              }
          }

          stage('Drop the Apache Tomcat Docker container') {
              steps {
                  echo 'dropping the container...'
                  sh 'docker rm -f ${CONTAINER_NAME} || true'
              }
          }

          stage('Copy the web application to the container directory') {
              steps {
                  echo 'Copying web application...'
                  sh 'test -d shopping'
                  sh 'cp -a shopping/. ${HOST_WEB_ROOT}/${APP_NAME}/'
                  sh 'ls -la ${HOST_WEB_ROOT}/${APP_NAME}'
              }
          }

          stage('Create the Tomcat container') {
              steps {
                  echo 'Creating the container...'
                  sh 'docker run -dit --name ${CONTAINER_NAME} -p 9090:8080 -v ${HOST_WEB_ROOT}:/usr/local/tomcat/webapps tomcat:9.0'
              }
          }
      }

      post {
          success {
              echo 'the deployment has worked'
          }
          failure {
              echo 'An error has ocurred'
          }
      }
  }
