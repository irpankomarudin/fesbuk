env.DOCKER_REGISTRY = 'rizaleko'
env.DOCKER_IMAGE_NAME = 'pesbuk'
pipeline {
    agent any
    stages {
        stage('build') {
            steps {
                sh('sed -i "s/tag/$BUILD_NUMBER/g" index.php')
                }
            }
        stage('docker build') {
            steps {
                sh "docker build --build-arg APP_NAME=$DOCKER_IMAGE_NAME -t $DOCKER_REGISTRY/$DOCKER_IMAGE_NAME ."
                }
           }
        stage('Docker push') {
            steps {                
                sh "docker push $DOCKER_REGISTRY/$DOCKER_IMAGE_NAME"
                }
           }
        stage('tagging') {
            steps {
                sh('sed -i "s/tag/$BUILD_NUMBER/g" deployment-fb.yml')
                }
           }
        stage('locate namespace') {
            steps {
              sh('sed -i "s/default/staging/g" deployment-fb.yml')
                }
           }
        stage('add domain') {
            steps {
                sh('sed -i "s/pesbuk.ridjal.com/spesbuk.ridjal.com/g" deployment-fb.yml')
                }
           }
        stage('deplo') {
            steps {
                sh('kubectl delete -f deployment-fb.yml')
                }
           }
        stage('deploy') {
            steps {
                sh('kubectl apply -f deployment-fb.yml')
                }
           }
        stage('remove image docker') {
            steps {
                sh "docker rmi $DOCKER_REGISTRY/$DOCKER_IMAGE_NAME"
                }
           }
         stage('show ingress') {
            steps {
                sh('kubectl get ingress -n=staging')
                }
           }        
      }
}
