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
  post {
    success {
      sh "echo 'Send mail on success'"
      // mail to:"puneet.sharma@firminiq", subject:"SUCCESS: ${currentBuild.fullDisplayName}", body: "Yay, we passed."
    }
    failure {
      sh "echo 'Send mail on failure'"
      // mail to:"puneet.sharma@firminiq.com", subject:"FAILURE: ${currentBuild.fullDisplayName}", body: "Boo, we failed."
    }
  }
}
