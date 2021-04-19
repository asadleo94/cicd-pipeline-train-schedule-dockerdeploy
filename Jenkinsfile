pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo 'Running build automation'
                sh './gradlew build --no-daemon'
                archiveArtifacts artifacts: 'dist/trainSchedule.zip'
            }
        }
        stage('Build Docker Image') {
            when {
                branch 'master'
            }
            steps {
                script {
                    app = docker.build("cloudyrion/train-schedule")
                    app.inside {
                        sh 'echo $(curl localhost:8080)'
                    }
                }
            }
        }
        stage('Push Docker Image') {
            when {
                branch 'master'
            }
            steps {
                script {                    
                    sh 'rm  ~/.dockercfg || true'
                    sh 'rm ~/.docker/config.json || true'
        
        //configure registry
        docker.withRegistry('https://173151801028.dkr.ecr.eu-central-1.amazonaws.com/cloudyrion_images', 'AWS_Login') {
          
            //build image
            def customImage = docker.build("my-image:${env.BUILD_ID}")
            
            //push image
            customImage.push()                    
                    }   
                }
        }
    }
}
