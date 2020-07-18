pipeline {
   agent any

   stages {
      stage('Verify Branch') {
         steps {
            echo "$GIT_BRANCH"
         }
      }
      stage('Docker Build') {
         steps {
            sh(script: 'docker images -a')
            sh(script: """
               cd azure-vote/
               sudo docker images -a
               sudo docker build -t jenkins-pipeline .
               sudo docker images -a
               cd ..
            """)
         }
      }
      stage('Start test app') {
         steps {
            sh(script: """
               docker-compose up -d
               ./scripts/test_container.ps1
            """)
         }
         post {
            success {
               echo "App started successfully :)"
            }
            failure {
               echo "App failed to start :("
            }
         }
      }
      
      stage('Run Tests') {
         steps {
            pwsh(script: """
               pytest ./tests/test_sample.py
            """)
         }
      }
      stage('Stop test app') {
         steps {
            pwsh(script: """
               docker-compose down
            """)
         }
      }
   }
}
