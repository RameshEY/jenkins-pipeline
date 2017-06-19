node {
  checkout scm
  env.PATH = "${tool 'Maven'}/bin:${env.PATH}"
  env.PATH = "${tool 'docker'}/bin:${env.PATH}"
  stage('Package') {
    dir('webapp') {
      sh 'mvn clean package -DskipTests'
    }
  }

  stage('Create Docker Image') {
    dir('webapp') {
      sh 'USER root'
      //sh 'sudo usermod -a -G docker $USER'
      //docker.build("deepakpanda1/jenkins-pipeline:${env.BUILD_NUMBER}")
    docker.build("test")
      
    }
  }

  stage('Run Tests') {
    try {
      dir('webapp') {
        sh "mvn test"
        docker.build("deepakpanda1/jenkins-pipeline:${env.BUILD_NUMBER}").push()
      }
    } catch (error) {

    } finally {
      junit '**/target/surefire-reports/*.xml'
    }
  }
}
