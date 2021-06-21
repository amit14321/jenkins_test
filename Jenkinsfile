pipeline {
    agent any
    
    //to enable triggeres schedule uncomment but we have to remove webohhopk below as it takes precedence
     //  triggers {
       // cron("*/2 * * * *")
    //}

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "MVN3"
        jdk "JDK1.8"
    }

    stages {
        stage('enable webhook') {
            steps {
                script {
                    properties([pipelineTriggers([githubPush()])])
                }
            }
        }
        
        stage('pullscm') {
            steps {
                     git credentialsId: '13400340-94b4-4004-9e0a-d47507f91053', url: 'git@github.com:amit14321/jenkins_test.git'
            }
        }
        stage('Build') {
            steps {
                // Run Maven on a Unix agent.
                sh "mvn -Dmaven.test.failure.ignore=true -f api-gateway clean package"

                // To run Maven on a Windows agent, use
                //bat "mvn -Dmaven.test.failure.ignore=true -f api-gateway clean package"
            }
            post {
                // If Maven was able to run the tests, even if some of the test
                // failed, record the test results and archive the jar file.
                success {
                    junit 'api-gateway/target/surefire-reports/*.xml'
                    archiveArtifacts 'api-gateway/target/*.jar'
                }
            }
        }
          stage('Email') {
            steps{
                script {
                    cest = TimeZone.getTimeZone("CEST")
                    def cest = new Date()
                    println(cest) 
                    def mailRecipients = 'amitsri491@gmail.com'
                    def jobName = currentBuild.fullDisplayName
                    env.Name = Name
                    env.cest = cest
                    emailext body: '''${SCRIPT, template="email-html.template"}''',
                    mimeType: 'text/html',
                    subject: "[Jenkins] ${jobName}",
                    to: "${mailRecipients}",
                    replyTo: "${mailRecipients}"
                           }
                        }
                    }
    }
}
