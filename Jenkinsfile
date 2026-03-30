  pipeline {
      agent any

      environment {
          JENKINS_WEB_ROOT = '/opt/tomcat-web'
          HOST_WEB_ROOT = '/home/mcherlo/docker/jenkins/tomcat-web'
          CONTAINER_NAME = 'tomcat1'
          APP_NAME = 'shopping'
      }

      stages {
          stage('Prepare directory for the web application') {
              steps {
                  echo 'Preparing deployment directory...'
                  sh 'mkdir -p ${JENKINS_WEB_ROOT}/${APP_NAME}'
                  sh 'find ${JENKINS_WEB_ROOT}/${APP_NAME} -mindepth 1 -maxdepth 1 -exec rm -rf {} +'
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
                  echo 'Debugging paths...'
                  sh 'pwd'
                  sh 'ls -la'
                  sh 'ls -la shopping || true'
                  sh 'ls -la ${JENKINS_WEB_ROOT} || true'

                  echo 'Copying web application...'
                  sh 'test -d shopping'
                  sh 'cp -a shopping/. ${JENKINS_WEB_ROOT}/${APP_NAME}/'
                  sh 'ls -la ${JENKINS_WEB_ROOT}/${APP_NAME}'
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
