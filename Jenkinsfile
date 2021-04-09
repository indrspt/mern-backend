env.DOCKER_REGISTRY = 'indraraspati'
env.DOCKER_IMAGE_NAME = 'mern-backend'
node('master') {
	stage('Mern Backend') {
      echo 'Mern Backend'
    }
    stage('Git Clone from Github') {
        git url: 'https://github.com/indrspt/mern-backend.git'
    }
    stage('Build Docker Image') {
        sh "docker build -t $DOCKER_REGISTRY/$DOCKER_IMAGE_NAME:${BUILD_NUMBER} ."
    }
    stage('Push Docker Image to Docker Hub') {
        sh "docker push $DOCKER_REGISTRY/$DOCKER_IMAGE_NAME:${BUILD_NUMBER}"
    }
    stage('Deploy To Kubernetes Cluster') {
        sh'''sed -i "20d" backend-deploy.yml'''
        sh'''sed -i "19 a \'\\'        image: indraraspati/mern-backend:${BUILD_NUMBER}" backend-deploy.yml && sed -i "s/''//"  backend-deploy.yml'''
        sh "kubectl apply -f  backend-deploy.yml"
   }
    stage('Remove Docker Image in local') {
        sh "docker rmi $DOCKER_REGISTRY/$DOCKER_IMAGE_NAME:${BUILD_NUMBER}"
    }
    stage("Clean Workspace"){
        cleanWs()
    }
}
