pipeline {
    
    agent {
        label 'Slave1'
    }

    tools 
    {
        maven 'maven-3.8.7'
    }
    
    stages {
        stage('SCM-Checkout') {
            steps {
                // Get some code from a GitHub repository
                git 'https://github.com/manju65char/Java-mvn-app2.git'

            }
              post {
                failure {
                  sh "echo 'Send mail on failure'"
                  mail to:"dummyid@gmail.com", from: 'dummyid@gmail.com', subject:"FAILURE: ${currentBuild.fullDisplayName}", body: "we failed."
                }
              }
			}
        stage('Build') {
            steps {
                // Run Maven on a Unix agent.
                sh "mvn -Dmaven.test.failure.ignore=true clean package"
            }
              post {
                failure {
                  sh "echo 'Send mail on failure'"
                  mail to:"dummyid@gmail.com", from: 'dummyid@gmail.com', subject:"FAILURE: ${currentBuild.fullDisplayName}", body: "Build failed."
                }
              }
			}

        stage('Deploy to QA AppServer') {
            steps {
				script {
                                         sshPublisher(publishers: [sshPublisherDesc(configName: 'QA_server', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: '', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '.', remoteDirectorySDF: false, removePrefix: 'target/', sourceFiles: 'target/maven-helloworld.war')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])				}
            }
              post {
                success {
                  sh "echo 'Send mail on success'"
                  //mail bcc: '', body: 'success', cc: '', from: '', replyTo: '', subject: 'success', to: 'dummyid@gmail.com'
                  mail to:"dummyid@gmail.com", from: 'dummyid@gmail.com', subject:"SUCCESS: ${currentBuild.fullDisplayName}", body: "we passed."
                }
                failure {
                  sh "echo 'Send mail on failure'"
                  mail to:"dummyid@gmail.com", from: 'dummyid@gmail.com', subject:"FAILURE: ${currentBuild.fullDisplayName}", body: "we failed."
                }
              }	
		}
    }
}
