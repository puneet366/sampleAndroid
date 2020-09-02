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
 notifyStarted()

    /* ... existing build steps ... */
    notifySuccessful()
         {
         } 
catch (err) 
{
    currentBuild.result = "FAILED"
    notifyFailed()
    throw e
  }
}

def notifyStarted() { /* .. */ }

def notifySuccessful() { /* .. */ }

def notifyFailed() {
  slackSend (color: '#FF0000', message: "FAILED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")

  hipchatSend (color: 'RED', notify: true,
      message: "FAILED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})"
    )

  emailext (
      subject: "FAILED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
      body: """
FAILED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]':


        
Check console output at "${env.JOB_NAME} [${env.BUILD_NUMBER}]"

""",
      recipientProviders: [[$class: 'DevelopersRecipientProvider']]
    )
}
