node {
         try
         {
         /*List environment = [
        "GOOGLE_APPLICATION_CREDENTIALS=/home/ubuntu/fqdemo-service-credentials-key.json"
    ]*/

 

stage('Checkout') {
        // Pull the code from the repo
        checkout scm
         akjofhpjf[qif[=qiw={}{}{
    } 
        } catch (e) {
           currentBuild.result = "SUCCESS"
           notifySUCCESS()
           throw e
    }
    finally
    {
        stage('Email')
        {
            env.ForEmailPlugin = env.WORKSPACE      
            emailext attachmentsPattern: 'TestResults\\*.trx',      
            body: '''${SCRIPT, template="groovy_html.template"}''', 
            subject: currentBuild.currentResult + " : " + env.JOB_NAME, 
            to: 'puneet.sharma@firminiq.com,sharma.shishu16@gmail.com'
        }
    }
}
