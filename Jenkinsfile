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
    stage('Email Notification'){
        mail bcc: '', body: '''Hi Welcome to jenkins email alerts
        Thanks
        Putra''', cc: '', from: '', replyTo: '', subject: 'Jenkins Job', to: 'iputusaputra.mitrais@gmail.com'
    }
}