node {
    def app
    def source
    stage('Build Docker Image') {
        source = checkout(scm)
        app = docker.build("mrnim94/sample-app:${env.BUILD_ID}", "--label dockerhp.sample.commit=${source.GIT_COMMIT} .")
    }

    stage('Publish to Docker Hub') {
        docker.withRegistry("https://index.docker.io/v1/", "dockerhub") {
            app.push(env.BUILD_ID)
        }
    }

    stage('Deploy to Production') {
        docker.withServer('tcp://192.168.21.21:2375', 'production') {
            sh "docker service update --image mrnim94/sample-app:${env.BUILD_ID} sample"
        }
    }
}
