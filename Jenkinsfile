pipeline {
    agent any

    stages {
        stage('Continuous Download') {
            steps {
                echo 'Downloading in progress...'
                git branch: 'main', url: 'https://github.com/chamberlain96/registration-app.git'
            }
        }

        stage('Continuous Build') {
            steps {
                echo 'Building in progress...'
                sh 'mvn clean install'
            }
        }

        stage('Code Quality Analysis') {
            steps {
                echo 'Performing code quality analysis...'
                sh 'mvn sonar:sonar'
            }
        }

    

        stage('Upload to Nexus') {
            steps {
                echo 'Uploading artifacts to Nexus...'
                // Replace with actual command to upload artifacts to Nexus
                sh 'mvn deploy'
            }
        }

        stage('Email Notification') {
            steps {
                echo 'Sending email notification...'
                emailext body: '''Hey team, please sign in here: ${BUILD_URL} to review and approve the build for deployment to UAT environment for testing.
                Thanks, Travis''', 
                subject: 'Build Notification - ${JOB_NAME}', 
                to: 'chamberlainaws@gmail.com,mbakuchamberlain@gmail.com,ngohneville200@gmail.com,preciousbradley64@gmail.com,preciousbradley64@gmail.com'
            }
        }

        stage('Approval') {
            steps {
                input message: 'Proceed to Deployment?', ok: 'Approve'
            }
        }

        stage('Continuous Deployment to UAT') {
            steps {
                echo 'Deploying to UAT...'
                deploy adapters: [tomcat9(credentialsId: 'tomcat', path: '', url: 'http://18.234.165.67:8080/')], contextPath: 'travisapp', war: '**/*.war'
            }
        }

        stage('Wait for 4 minutes') {
            steps {
                echo 'Waiting for  minutes before deploying to production...'
                sleep(time: 1, unit: 'MINUTES')
            }
        }

        stage('Continuous Deployment to Prod') {
            steps {
                echo 'Deploying to Prod...'
               deploy adapters: [tomcat9(credentialsId: 'tomcat', path: '', url: 'http://35.171.157.175:8080/')], contextPath: 'travisap', war: '**/*.war'
            }
        }
    }

    post {
        success {
            emailext body: 'Hey team, DEPLOYMENT was successful! Check details here: ${BUILD_URL}', 
            subject: 'SUCCESS - ${JOB_NAME}', 
            to: 'chamberlainaws@gmail.com,mbakuchamberlain@gmail.com,ngohneville200@gmail.com,preciousbradley64@gmail.com,preciousbradley64@gmail.com'
        
        }
        
        failure {
            emailext body: 'Hey team, the build FAILED. Check details here: ${BUILD_URL} and fix errors ASAP.', 
            subject: 'FAILURE - ${JOB_NAME}', 
            to: 'chamberlainaws@gmail.com,ngohneville200@gmail.com,preciousbradley64@gmail.com,preciousbradley64@gmail.com'
            
        }
    }
}
