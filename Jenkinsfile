node ('linux') {
try {
stage('Checkout') {
checkout scm
}
stage('Lint') {
sh 'ant lint'
}
stage('Build') {
sh 'ant composer.install'
}
stage('Code Analysis') {
sh 'ant sonar'
}
stage('Deploy') {
sh 'ant permissions.set'
sh 'ant deploy'
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


