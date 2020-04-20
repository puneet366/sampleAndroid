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
        keyStoreId: "d8791f5a-e4eb-4bf2-a50c-ef45289a2708",
        keyAlias: "key0",
        skipZipalign: true,
        apksToSign: "**/*.apk"
        
        )
    }


stage('Archive')  {

             archiveArtifacts artifacts: '**/*-signed.apk',  allowEmptyArchive: false
             archiveArtifacts artifacts: '**/app-release.apk',  allowEmptyArchive: false
    } 


stage ('Distribute') {

            /*withEnv(environment) {
                  
                   //sh "./gradlew assembleRelease appDistributionUploadRelease"
                  sh """./gradlew assembleDebug appDistributionUploadDebug
              ./gradlew assembleRelease appDistributionUploadRelease
           """ 
                  
                
                  }  */
    
    
   sh ' firebase appdistribution:distribute /var/lib/jenkins/workspace/android-automation/app/build/outputs/apk/release/app-release.apk --app 1:444141390138:android:c4984d1c92b288f4466803 --release-notes "app-release.apk" --groups "fq-testers" '
   sh ' firebase appdistribution:distribute /var/lib/jenkins/workspace/android-automation/app/build/outputs/apk/debug/*-signed.apk --app 1:444141390138:android:c4984d1c92b288f4466803 --release-notes "app-debug.apk" --groups "fq-testers" '
         
    
    
            

          }


}
