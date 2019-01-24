node('master') {
  stage('Poll') {
    checkout scm
  }
  stage('Build & Unit test'){
    sh 'mvn clean verify -DskipITs=true';
    junit '**/target/surefire-reports/TEST-*.xml'
    archiveArtifacts 'target/*.jar'
  }
  stage ('Integration Test'){
    sh 'mvn clean verify -Dsurefire.skip=true';
    junit '**/target/failsafe-reports/TEST-*.xml'
    archiveArtifacts 'target/*.jar'
  }
  stage ('Publish'){
    def server = Artifactory.server 'Study Artifactory Server'
    def uploadSpec = """{
      "files": [
        {
          "pattern": "target/*.jar",
          "target": "jenkins_test_demo/${BUILD_NUMBER}/",
          "props": "Integration-Tested=Yes;Performance-Tested=No"
        }
      ]
    }"""
    server.upload(uploadSpec)
  }
   stage('Deploy') {
        echo "Deploy is not yet implemented"
		//sh "ssh root@172.17.201.150 rm -rf /root/nems2/jenkins_deploy_test/dist/"
		sh "ssh root@172.17.201.150 mkdir -p /root/nems2/jenkins_deploy_test/"
		//sh ‘scp -r dist root@172.17.201.150:/root/nems2/jenkins_deploy_test/disk/’
    }
}
