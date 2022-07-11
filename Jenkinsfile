pipeline {
  agent any
  stages {
    stage('Build') {
      parallel {
        stage('Build') {
          steps {
            sh 'echo "building the repo"'
            export CONDUIT_SECRET='KeyboarD@2525'
            export FLASK_APP=autoapp.py
            export FLASK_DEBUG=1
            sh ´pip install -r requirements/dev.txt
          }
        }
      }
    }
  
    stage('Test') {
      steps {
        sh 'python3 autoapp.py'
      }
    }
  
    stage('Deploy')
    {
      steps {
        echo "deploying the application"
        sh "sudo nohup python3 autoapp.py > log.txt 2>&1 &"
      }
    }
  }
  
  post {
        always {
            echo 'The pipeline completed'
            flask db init
            flask db migrate
            flask db upgrade
            junit allowEmptyResults: true, testResults:'**/test_reports/*.xml'
        }
        success {                   
            echo "Flask Application Up and running!!"
        }
        failure {
            echo 'Build stage failed'
            error('Stopping early…')
        }
      }
}
