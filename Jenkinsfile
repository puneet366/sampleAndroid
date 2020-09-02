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
    post {

        failure {
            mail to: 'user@mail.com',
            subject: "Failed Pipeline: ${currentBuild.fullDisplayName}",
            body: "Build failed: ${env.BUILD_URL}"
        }

        success {
            if (currentBuild.previousBuild != null && currentBuild.previousBuild.result != 'SUCCESS') {
                mail to: 'user@mail.com',
                subject: "Pipeline Success: ${currentBuild.fullDisplayName}",
                body: "Build is back to normal (success): ${env.BUILD_URL}"
        }
    }
}
