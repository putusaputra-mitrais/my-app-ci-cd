node{
    stage('SCM Checkout'){
        git 'https://github.com/putusaputra-mitrais/my-app-ci-cd'
    }
    stage('Compile-Package'){
        sh 'mvn package'
    }
}