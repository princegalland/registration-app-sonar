pipeline {
    agent any

    stages {
         stage('continuos download') {
            steps {
                echo 'echo dowloading in progress'
                git branch: 'main', url: 'https://github.com/chamberlain96/registration-app.git'
            }
    }
         stage('continuos build') {
            steps {
                echo 'echo building in progress'
                 sh 'mvn clean install '
    
        }
    }
         stage('Approval') {
            steps {
                input message: 'Proceed to Deployment?', ok: 'Approve'
            }
        }
        
         stage('continuos delopment') {
            steps {
                echo 'echo deploying application'
                deploy adapters: [tomcat9(credentialsId: 'tomcat', path: '', url: 'http://54.86.63.197:8080/')], contextPath: 'travisapp', war: '**/*.war'
}
            }
    
}
}
