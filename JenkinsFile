pipeline {
    agent {
        label "slave"
    }

    tools {
        maven 'Maven3'
    }
    environment {
        registry = "akdevopscoaching/springbootapp"
        registryCredential = "dockerhub"
        def image = ''
    }
    
    stages {
        stage('Checkout') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://bitbucket.org/ananthkannan/myawesomeangularapprepo']]])    
            }
        }
        
    stage ('Build') {
        steps {
            sh 'mvn -f MyAwesomeApp/pom.xml clean install'           
        }
    }
        
    stage ('Docker Build') {
        steps {
            script 
            {
         // Build and push image with Jenkins' docker-plugin
            withDockerRegistry([credentialsId: "dockerhub", url: "https://index.docker.io/v1/"]) {
            image = docker.build("akdevopscoaching/springbootapp", "MyAwesomeApp")
            image.push()    
            }
            }
        }
    }
    
      stage ('K8S Deploy') {
       steps {
                kubernetesDeploy(
                    configs: 'MyAwesomeApp/springboot-docker-hub.yaml',
                    kubeconfigId: 'K8S',
                    enableConfigSubstitution: true
                    )               
        }
      }
  }
}
