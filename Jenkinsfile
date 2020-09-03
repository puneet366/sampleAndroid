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
         } catch (e) {
           currentBuild.result = "FAILED"
           notifyFailed()
           throw e
    }
    finally
    {
    }
}
def notifyBuild(String buildStatus = 'STARTED') {
  // build status of null means successful
  buildStatus = buildStatus ?: 'SUCCESS'

  // Default values
  def colorName = 'RED'
  def colorCode = '#FF0000'
  def subject = "${buildStatus}: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'"
  def summary = "${subject} (${env.BUILD_URL})"
  def details = """<p>STARTED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]':</p>
    <p>Check console output at &QUOT;<a href='${env.BUILD_URL}'>${env.JOB_NAME} [${env.BUILD_NUMBER}]</a>&QUOT;</p>"""

  // Override default values based on build status
  if (buildStatus == 'STARTED') {
    color = 'YELLOW'
    colorCode = '#FFFF00'
  } else if (buildStatus == 'SUCCESS') {
    color = 'GREEN'
    colorCode = '#00FF00'
  } else {
    color = 'RED'
    colorCode = '#FF0000'
  }
        stage('Email')
        {
            env.ForEmailPlugin = env.WORKSPACE      
            emailext attachmentsPattern: 'TestResults\\*.trx',      
            body: '''${SCRIPT, template="groovy_html.template"}''', 
            subject: currentBuild.currentResult + " : " + env.JOB_NAME, 
            to: 'puneet.sharma@firminiq.com'
        }
    }
}
