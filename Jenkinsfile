node {
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
  
  stage('Email Notofication')  {
    mail bcc: '', body: 'Hi Welcome to Jenkins email notification', cc: '', from: '', replyTo: '', subject: 'Jenkins email notification', to: 'puneet.sharma@firminiq.com'
    
  }
  
}
