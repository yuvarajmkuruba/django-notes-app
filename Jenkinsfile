pipeline {
    agent any 
    stages {
        stage("Code"){
            steps {
                echo "Cloning te code"
                git url: "https://github.com/yuvarajmkuruba/django-notes-app.git" ,  branch: "main"
            }
            
        }
        stage("Build"){
              steps {
                 echo "Building the Code"
                 sh "docker build -t mynotes . "
            }
            
        }
        stage ("Push to Docker Hub"){
              steps {
                echo "Pushing  images to Docker Hub "
                withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'dockerhubPass', usernameVariable: 'dockerhubUser')]) {
                    sh "docker tag mynotes  ${dockerhubUser}/mynotes:latest "
                    sh "docker login -u ${dockerhubUser} -p ${dockerhubPass}"
                    sh "docker  push ${dockerhubUser}/mynotes:latest "
                }
            }
        }
        stage("Deploy"){
               steps {
                echo "Deploy ....."
                sh "docker-compose down && docker-compose up -d "
                // sh " docker run -d -p 8000:8000  yuvarajmcloud/mynotes:latest"
            }
        }
    }
}
