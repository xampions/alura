    import hudson.model.*
    node {
          wrap([$class: 'TimestamperBuildWrapper']) {
               stage ('Checkout')
               deleteDir()
               checkout scm
                bat 'del /F /Q outfile '
                bat 'git name-rev --name-only HEAD > outfile'
                def Branch_Name = readFile 'outFile'
                println Branch_Name

                
                def Branch_Master = "remotes/origin/master"
                Branch_Name = Branch_Name.trim()
                Branch_Master = Branch_Master.trim()


/*
                if (Branch_Name == Branch_Master) {
                stage('SonarQube Analysis') {
                def pom = readMavenPom()
                writeFile encoding: 'UTF-8', file: 'sonar-project.properties', text: """
                # must be unique in a given SonarQube instance
                sonar.projectKey=$pom.groupId:$pom.artifactId
                sonar.projectName=$pom.artifactId
                sonar.projectVersion=$pom.version
                sonar.genericcoverage.reportPaths=target/surefire-reports/TEST-jenkins.plugins.jsonHelper.FromJsonStepTest.xml
                sonar.sourceEncoding=UTF-8
                sonar.java.source=1.7
                # Path is relative to the sonar-project.properties file. Replace "\\" by "/" on Windows.
                # Since SonarQube 4.2, this property is optional if sonar.modules is set.
                # If not set, SonarQube starts looking for source code from the directory containing
                # the sonar-project.properties file.
                sonar.sources=."""
                
                archive 'sonar-project.properties'                
                // requires SonarQube Scanner 2.8+
                def jdk8 = tool name: '1.8.0_101'
                env.JAVA_HOME = "${jdk8}"
                def scannerHome = 'C:\\sonar-scanner-2.8';
                withSonarQubeEnv('5.6.6') {
                //sh "${scannerHome}/bin/sonar-scanner"
            	bat "${scannerHome}\\bin\\sonar-scanner.bat"
                  }
                }
                                    
                                    }    
*/
               stage ('Build')
                                  def jdk = tool name: '1.7.0_79'
                def mvnHome = tool name: '3.3.9'
            	env.JAVA_HOME = "${jdk}"
            	bat "${mvnHome}/bin/mvn clean install"


               stage ('Warnings-Publisher')
                warnings canComputeNew: false, canResolveRelativePaths: false, consoleParsers: [[parserName: 'Java Compiler (javac)'], [parserName: 'JavaDoc Tool'], [parserName: 'Maven']], defaultEncoding: '', excludePattern: '', healthy: '', includePattern: '', messagesPattern: '', unHealthy: '' 
               
               stage ('Archive')
                          	step([$class: 'ArtifactArchiver', artifacts: '**/*.war', fingerprint: true])
                   
               stage ('Delete Workpspace')
                   deleteDir()

    	  }
    }
