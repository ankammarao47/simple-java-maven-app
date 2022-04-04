pipeline {
    agent any

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "maven1"
    }

    stages {
        stage('hello'){
            steps{
                echo 'hello world'
            }
        }
        
        stage('Build') {
            steps {
                // Get some code from a GitHub repository
                //git 'https://github.com/jglick/simple-maven-project-with-tests.git'
                 checkout([$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[credentialsId: 'githubtoken', url: 'https://github.com/ankammarao47/simple-java-maven-app.git']]])
                // Run Maven on a Unix agent.
                bat "mvn -Dmaven.test.failure.ignore=true clean package"

                // To run Maven on a Windows agent, use
                // bat "mvn -Dmaven.test.failure.ignore=true clean package"
            }
        }

           
            stage('Test') {
                 steps {

               bat 'mvn test -Dmaven.test.failure.ignore=true'
                   }
                }
                stage('sonar') {
             steps {

                bat 'mvn sonar:sonar -Dsonar.login=f3811202efe8a39190d20b74da68b6b2f9b8faa5'
                     }
               }
               stage('nexus'){
                   steps{
                      nexusArtifactUploader artifacts: [[artifactId: 'pom.my-app', classifier: '', file: 'pom.xml', type: 'pom.jar']], credentialsId: 'nexusCR', groupId: 'pom.com.mycompany.app', nexusUrl: 'localhost:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'mavenjar', version: 'pom.1.0-SNAPSHOT'

                   }
               }
        }
    }
