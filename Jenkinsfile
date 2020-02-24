node {
    
    List environment = [

        "GOOGLE_APPLICATION_CREDENTIALS=/home/ubuntu/fqdemo-service-credentials-key.json"

    ]

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

             archiveArtifacts artifacts: 'app/build/outputs/apk/debug/*-signed.apk', fingerprint: false, allowEmptyArchive: false
             archiveArtifacts artifacts: 'app/build/outputs/apk/release/app-release.apk', fingerprint: false, allowEmptyArchive: false
    }


stage ('Distribute') {

            /*withEnv(environment) {
                  
                   //sh "./gradlew assembleRelease appDistributionUploadRelease"
                /*  sh """./gradlew assembleDebug appDistributionUploadDebug
              ./gradlew assembleRelease appDistributionUploadRelease
           """ */
                  
                
                  } */
    
    
                steps {
                
                sh "firebase appdistribution:distribute '/var/lib/jenkins/workspace/fqsample1_masterapp/build/outputs/apk/release/app-release.apk' --app 1:444141390138:android:c4984d1c92b288f4466803 --release-notes "Bug fixes and improvements" --testers "nikhil.vyas@firminiq.com" "
              }
    
                steps {
         bash '''#!/bin/bash
                 firebase appdistribution:distribute '/var/lib/jenkins/workspace/fqsample1_masterapp/build/outputs/apk/release/app-release.apk' --app 1:444141390138:android:c4984d1c92b288f4466803 --release-notes "Bug fixes and improvements" --testers "nikhil.vyas@firminiq.com"
         '''
    }
    
            

          }


}
