properties([pipelineTriggers([githubPush()])])

// external functions
void setBuildStatus(String message, String state) {
    step([
        $class: "GitHubCommitStatusSetter",
        reposSource: [$class: "ManuallyEnteredRepositorySource", url: "https://github.com/putusaputra-mitrais/my-app-ci-cd"],
        contextSource: [$class: "ManuallyEnteredCommitContextSource", context: "ci/jenkins/build-status"],
        errorHandlers: [[$class: "ChangingBuildStatusErrorHandler", result: "UNSTABLE"]],
        statusResultSource: [ $class: "ConditionalStatusResultSource", results: [[$class: "AnyBuildResult", message: message, state: state]] ]
    ]);
}

node{
    try {
        // this will run only if successful
        stage('SCM Checkout'){
            git 'https://github.com/putusaputra-mitrais/my-app-ci-cd'
        }
        stage('Compile-Package'){
            sh 'mvn package'
        }
        stage('SonarQube Analysis'){
            withSonarQubeEnv('sonar-6'){
                sh "mvn sonar:sonar"
            }
        }
        stage("Quality Gate Status Check"){
            timeout(time: 1, unit: 'HOURS') {
                def qg = waitForQualityGate()
                if (qg.status != 'OK') {
                    // Email notification if quality gate failed
                    mail bcc: '', body: '''SonarQube Analysis Failed''',
                    cc: '', from: '', replyTo: '', subject: 'Jenkins Job : Sonarqube checks failed', to: 'iputusaputra.mitrais@gmail.com'

                    // mark this build as error and abort next pipeline stage
                    error "Pipeline aborted due to quality gate failure: ${qg.status}"
                }
            }
        }
        stage("Deploy jar file"){
            sh 'cp target/gs-maven-0.1.0-shaded.jar D:/Putra/CDC/team-7/test-deploy-jenkins'
            echo "${GIT_COMMITTER_EMAIL}"
        }  
        stage('Email Notification'){
            mail bcc: '', body: '''Hi Welcome to jenkins email alerts
            Thanks
            Putra''', cc: '', from: '', replyTo: '', subject: 'Jenkins Job', to: 'iputusaputra.mitrais@gmail.com'
        }

        currentBuild.result = 'SUCCESS'
    } catch (e) {
        // this will run only if failed
        currentBuild.result = 'FAILURE'
        throw e
    } finally {
        // this will always run
        def buildStatusMessage = ''
        if (currentBuild.result == 'SUCCESS') {
            buildStatusMessage = 'Build complete'
        } else {
            buildStatusMessage = 'Build failed'
        }

        setBuildStatus(buildStatusMessage, currentBuild.result);
    }
}