node {
    def application = "pythonapp6"
    def dockerhubaccountid = "erisjat"
    stage('Clone repository') {
        checkout scm
    }

    stage('Build image') {
        app = docker.build("${dockerhubaccountid}/${application}:${BUILD_NUMBER}")
    }

    stage('Push image') {
        withDockerRegistry([ credentialsId: "jenkins-hubdocker", url: "" ]) {
        app.push()
        app.push("latest")
    }
    }

    stage('Deploy') {
        sh ("docker ps -f name=python -q | xargs --no-run-if-empty docker container stop")
        sh ("docker container ls -a -fname=python -q | xargs -r docker container rm")
        sh ("docker run -p 5000:5000 --name python -d ${dockerhubaccountid}/${application}:${BUILD_NUMBER}")
    }

    stage('Remove old images') {
        // remove old docker images
        sh("docker rmi ${dockerhubaccountid}/${application}:latest -f")
   }
}