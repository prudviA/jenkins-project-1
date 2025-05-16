pipeline {
    agent any
    
    environment {
        IMAGE_NAME = 'springboot-thymeleaf.crud'
        CONTAINER_NAME ='cont1'
        APP_PORT = '8082'
}
 stages {
     stage('Clone Repo') {
       steps {
           git 'https://github.com/prudviA/jenkins-project-1.git'
        }
  }
 stage('Build with Maven') {
  steps {
    sh 'mvn clean install -DskipTests'
    }
}

 stage('Stop Existing container') {
 steps {
    scripts {
         sh """
         if [ \$(docker ps -q -f name=$CONTAINER_NAME) ]; then
         docker stop $CONTAINER_NAME
	 docker rm $CONTAINER_NAME
         fi
         """
       }
   }
}

     stage('Run Container') {
         steps {
              sh 'docker run -d -p 8081:$APP_PORT --name $CONTAINER_NAME $IMAGE_NAME'
         }
     }
}



post { 
    success {
         echo "Depolyment successfuk. App running on port 8082"
     }
    failure {
          echo "Something went wrong!"
     }
   }
}
