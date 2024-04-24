pipeline {
    agent any
    options{
        retry(3)
        timeout(time: 1, unit: 'MINUTES')
    }
        triggers{
            pollSCM('* * * * *')
        }
        tools {
            jdk 'JAVA_8'
        }
        parameters {
            choice(name: 'GOAL', choices:['package','clean package','validate','install'], 
            description: 'This is maven goal')

        }
        stages{
            stage('VCS'){
                steps{
                    git url: 'https://github.com/vijaysmetgud/game-of-life-july23.git',
                        branch: 'master'
                }
            }
            stage('build and package'){
                steps{
                    sh script: "mvn ${params.GOAL}"
                }
            }
            stage('archieve and publish junittest results'){
                steps{
                    archiveArtifacts artifacts: '**/target/gameoflife.war'
                    junit testResults: '**/surefire-reports/TEST-*.xml' 
                }
              
            }
           
        }

        post {
            success {
                mail subject: "${JOB_NAME}:has completed with success",
                     body: "Hi this jenkins job \n Build Url ${BUILD_URL}",
                     cc: 'ajaykumar.matters@gmail', 
                     from: 'info@test.com',
                     to: 'admin@jenkins.com'
            }
            
            failure {
                mail subject: "${JOB_NAME}:has completed with success",
                     body: "Hi this jenkins job \n Build Url ${BUILD_URL}",
                     cc: 'ajaykumar.matters@gmail', 
                     from: 'info@test.com',
                     to: 'admin@jenkins.com' 
            }
        }
}
