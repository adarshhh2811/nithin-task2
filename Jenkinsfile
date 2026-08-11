pipeline {

    agent any

    

    environment {
        IMAGE_NAME = "adarsh28111/nithin"
        IMAGE_TAG = "${BUILD_NUMBER}"
    }

    stages {

        stage('SCM Pull') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/adarshhh2811/nithin-task2.git'
            }
        }

    stage('Compile') {
            steps {
                sh 'mvn clean compile'
            }
        }

    stage('Package') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

   stage('Build Docker Image') { 
    steps { 
        sh """ 
            docker build -t "${IMAGE_NAME}:${IMAGE_TAG}" .
            docker tag "${IMAGE_NAME}:${IMAGE_TAG}" "${IMAGE_NAME}:latest"
        """ 
    } 
}


  stage('docker compose') { 
    steps { 
        // Use -f to specify your compose file path if it's in a subdirectory
        sh ''' 
            docker-compose down --remove-orphans || true
            docker-compose up -d --force-recreate --build
        ''' 
    } 
} 

stage('Docker Cleanup') { 
    steps { 
        // Changed image prune to system prune to clean up stopped containers, networks, and build cache
        sh ''' 
            docker system prune -f --volumes
            docker system df 
        ''' 
    } 
}

       
   }        
}
