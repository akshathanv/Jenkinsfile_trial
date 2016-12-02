node {
  stage 'Checkout'
  git url: 'https://github.com/akshathanv/Jpetstore_maven.git'   
  // Clean any locally modified files and ensure we are actually on origin/master
  // as a failed release could leave the local workspace ahead of origin/master
  sh "git clean -f && git reset --hard origin/master"
  
  def mvnHome = tool 'M3'
  stage 'Build'
  sh "${mvnHome}/bin/mvn clean install"
  
  //stage 'Run SonarQube analysis'
  //sh "${mvnHome}/bin/mvn sonar:sonar -Dsonar.host.url=http://16.181.237.15:9000/"
  
   def mvnHome = tool 'M3'
  stage 'Artifactory configuration'
        def server = Artifactory.newServer url: 'https://hpedocker.southeastasia.cloudapp.azure.com/artifactory/mavensnapshot', username: 'admin', password: 'password'
        //def server = Artifactory.server SERVER_ID
        def artifactoryMaven = Artifactory.newMavenBuild()
        //def mvnHome = tool 'M3'
        artifactoryMaven.tool = M3 // Tool name from Jenkins configuration
        artifactoryMaven.deployer snapshotRepo:'mavensnapshot', server: server
        //artifactoryMaven.resolver releaseRepo:'libs-release', snapshotRepo:'libs-snapshot', server: server
        def buildInfo = Artifactory.newBuildInfo()

    stage 'Exec Maven'
        artifactoryMaven.run pom: 'Jpetstore_maven/pom.xml', goals: 'clean install', buildInfo: 'buildInfo'
  
    stage 'Publish build info'
        server.publishBuildInfo buildInfo
  
  
}
