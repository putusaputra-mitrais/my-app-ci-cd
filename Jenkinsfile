node{
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
        sh 'copy ./target/gs-maven-0.1.0-shaded.jar D:/Putra/CDC/team-7/test-deploy-jenkins/'
    }  
    stage('Email Notification'){
        mail bcc: '', body: '''Hi Welcome to jenkins email alerts
        Thanks
        Putra''', cc: '', from: '', replyTo: '', subject: 'Jenkins Job', to: 'iputusaputra.mitrais@gmail.com'
    }
}