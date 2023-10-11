node {
    def GIT_URL = 'https://github.com/shashikanthmartha/java-maven-calculator-web-app.git'
    def SONARQUBE_SERVER = 'http://localhost:9000'

    try {
        stage('Checkout') {
            checkout([$class: 'GitSCM', branches: [[name: '*/main']], userRemoteConfigs: [[url: GIT_URL]]])
        }

        stage('Build') {
            sh 'mvn clean install'
        }

        stage('Static Code Analysis') {
            withSonarQubeEnv('SonarQube') {
                sh 'mvn sonar:sonar'
            }
        }

        stage('Deploy to Tomcat') {
            sh 'cp target/your-web-app.war $CATALINA_HOME/webapps/'
        }
    } catch (Exception e) {
        currentBuild.result = 'FAILURE'
        throw e
    } finally {
        stage('Post-build') {
            step([$class: 'JUnitResultArchiver', testResults: 'target/test-reports/**/*.xml'])
            step([$class: 'Mailer', recipients: 'shashikanthmartha@gmail.com', subject: "Build ${currentBuild.result}: Job '${env.JOB_NAME}'"])
        }
    }
}
