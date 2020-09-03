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
    stage('Acceptance') {
sh 'ant test.acceptance'
notifySuccessful()
} catch (e) {
notifyFailed()
throw e
}
}
def notifyFailed() {
hipchatSend (color: 'RED', notify: true,
message: "FAILED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' ($
{env.BUILD_URL})"
)
emailext (
subject: "FAILED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
body: """<p>FAILED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]':</p>
<p>Check console output at &QUOT;<a href='$
{env.BUILD_URL}/consoleText'>${env.JOB_NAME} [$
{env.BUILD_NUMBER}]</a>&QUOT;</p>""",
to: 'xxxxx'
)
}
def notifySuccessful() {
hipchatSend (color: 'GREEN', notify: true,
message: "SUCCESSFUL: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' ($
{env.BUILD_URL})"
)
}
