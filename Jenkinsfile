node {

    stage("Git Clone"){

        git credentialsId: 'github-ssh-oz', url: 'git@github.com:oznetit76/project1.6-k8s-aws.git'
    }

     stage('Gradle Build') {

       sh './gradlew build'

    }

    stage("Docker build"){
        sh 'docker version'
        sh 'docker build -t jhooq-docker-demo .'
        sh 'docker image list'
        sh 'docker tag jhooq-docker-demo 037624764/jhooq-docker-demo:jhooq-docker-demo'
    }

    withCredentials([string(credentialsId: 'DOCKER_HUB_PASSWORD', variable: 'PASSWORD',)]) {
        sh 'docker login -u 037624764 -p $PASSWORD'
    }

    stage('publish docker') {
        withCredentials([usernamePassword(credentialsId: 'DOCKER_HUB_PASSWORD', passwordVariable: 'DOCKER_HUB_PASSWORD', usernameVariable: 'DOCKER_REGISTRY_USER')]) {
        }
    }
    stage("Push Image to Docker Hub"){
        sh 'docker push  037624764/jhooq-docker-demo:jhooq-docker-demo'
    }
    
    stage("kubernetes deployment"){
        sh 'kubectl apply -f k8s-spring-boot-deployment.yml -n jhooq'
    }
} 
