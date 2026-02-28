pipeline {
    agent any

    stages {
        stage('Code Clone'){
            steps {
                git url:'https://github.com/robinvatshr08/notes.git', branch: 'main'
                echo 'Code cloned successfully'
            }
        }
        
        stage('Build Image') {
            steps {
                sh 'docker build -t notes:latest .'
                echo 'Build completed'
            }
        }

        stage('Push to Docker Hub'){
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'DockerCred',
                    usernameVariable: "dockerHubUser",
                    passwordVariable: "dockerHubPass"
                )]){
                    
                    sh '''
                    echo $dockerHubPass | docker login -u $dockerHubUser --password-stdin
                    docker tag notes:latest $dockerHubUser/notes:latest
                    docker push $dockerHubUser/notes:latest
                    '''
                    
                    echo 'Image pushed successfully'
                }
            }
        }
        
        stage('Deploy'){
            steps {
                sh 'docker compose up -d'
                echo 'Application deployed successfully'
            }
        }
    }
}
