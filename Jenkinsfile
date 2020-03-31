node {
    def app
    stage('Build Docker Image') {
        checkout scm
        app = docker.build('mrnim94/sample-app:latest')
    }
    
    stage('Publish to Docker Hub') {
        docker.withRegistry("https://index.docker.io/v1/", "dockerhub") {
            app.push('latest')
        }
    }

    stage('Deploy to Production') {
        docker.withServer('tcp://192.168.21.21:2375', 'production') {
            sh 'docker run -d mrnim94/sample-app'
        }
    }
}
