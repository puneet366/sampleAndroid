node {
         try
         {
         /*List environment = [
        "GOOGLE_APPLICATION_CREDENTIALS=/home/ubuntu/fqdemo-service-credentials-key.json"
    ]*/

 

stage('Checkout') {
        // Pull the code from the repo
         checkout scm{{
                  {}{}{
                           {}
    } 
        } catch (e) {
           currentBuild.result = "FAILED"
           notifyFailed()
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
            to: 'sharma.shishu16@gmail.com'
        }
    }
}
