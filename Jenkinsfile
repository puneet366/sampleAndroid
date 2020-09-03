node {
         try
         {
         /*List environment = [

        "GOOGLE_APPLICATION_CREDENTIALS=/home/ubuntu/fqdemo-service-credentials-key.json"
    ]*/

stage('Checkout') {

        // Pull the code from the repo

        checkout scm

    }

stage('Clean') {

        sh "./gradlew clean"
    }

    
stage('Build')  {

        //sh "./gradlew assembleRelease"
    
        sh """./gradlew assembleDebug
              ./gradlew assembleRelease
           """
    }
    
    
    
stage('sign apk')  {
        signAndroidApks (
        keyStoreId: "7e6fd0fe-ab86-4a12-b8ea-68b9c8b20a2d",
        keyAlias: "key0",
        skipZipalign: true,
        apksToSign: "**/*.apk"
        
        )
    }


stage('Archive')  {

             archiveArtifacts artifacts: '**/*-signed.apk',  allowEmptyArchive: false
             archiveArtifacts artifacts: '**/app-release.apk',  allowEmptyArchive: false
    } 
         }
    pipeline {
    environment {
        //This variable need be tested as string
        doError = '1'
    }
   
    agent any
    
    stages {
        stage('Error') {
            when {
                expression { doError == '1' }
            }
            steps {
                echo "Failure"
                error "failure test. It's work"
            }
        }
        
        stage('Success') {
            when {
                expression { doError == '0' }
            }
            steps {
                echo "ok"
            }
        }
    }
    post {
        always {
            echo 'I will always say Hello again!'
            
            emailext body: "${currentBuild.currentResult}: Job ${env.JOB_NAME} build ${env.BUILD_NUMBER}\n More info at: ${env.BUILD_URL}",
                recipientProviders: [[$class: 'DevelopersRecipientProvider'], [$class: 'RequesterRecipientProvider']],
                subject: "Jenkins Build ${currentBuild.currentResult}: Job ${env.JOB_NAME}"
            
        }
    }
