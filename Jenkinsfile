pipeline{
  agent any
  environment {
        FOO = "bar"
        NAME = "Joe"
    }


  stages{

    stage('Run Tests') {
            parallel {
                stage('Test On Windows') {
                    agent {
                        label "windows"
                    }
                    steps {
                        bat "run-tests.bat"
                    }
                    post {
                        always {
                            junit "**/TEST-*.xml"
                        }
                    }
                }
                stage('Test On Linux') {
                    agent {
                        label "linux"
                    }
                    steps {
                        sh "run-tests.sh"
                    }
                    post {
                        always {
                            junit "**/TEST-*.xml"
                        }
                    }
                }
            }
        }
    




    stage('Create build output and archive it'){
      steps{
            notify('Started')
            // Make the output directory.
            sh "mkdir -p output"

            // Write an useful file, which is needed to be archived.
            writeFile file: "output/usefulfile.txt", text: "This file is useful, need to archive it."

            // Write an useless file, which is not needed to be archived.
            writeFile file: "output/uselessfile.md", text: "This file is useless, no need to archive it."
           } 
     }


    stage ('Archive build output'){
      steps{
    
          // Archive the build output artifacts.
            archiveArtifacts artifacts: 'output/*.txt', excludes: 'output/*.md'
           }

     }

     stage("Env Variables") {
            environment {
                NAME = "Alan" // overrides pipeline level NAME env variable
                BUILD_NUMBER = "2" // overrides the default BUILD_NUMBER
            }

            steps {
                echo "FOO = ${env.FOO}" // prints "FOO = bar"
                echo "NAME = ${env.NAME}" // prints "NAME = Alan"
                echo "BUILD_NUMBER =  ${env.BUILD_NUMBER}" // prints "BUILD_NUMBER = 2"

                script {
                    env.SOMETHING = "1" // creates env.SOMETHING variable
                }
            }
        }


      stage("Override Variables") {
            steps {
                script {
                    env.FOO = "IT DOES NOT WORK!" // it can't override env.FOO declared at the pipeline (or stage) level
                    env.SOMETHING = "2" // it can override env variable created imperatively
                }

                echo "FOO = ${env.FOO}" // prints "FOO = bar"
                echo "SOMETHING = ${env.SOMETHING}" // prints "SOMETHING = 2"

                withEnv(["FOO=foobar"]) { // it can override any env variable
                    echo "FOO = ${env.FOO}" // prints "FOO = foobar"
                }

                withEnv(["BUILD_NUMBER=1"]) {
                    echo "BUILD_NUMBER = ${env.BUILD_NUMBER}" // prints "BUILD_NUMBER = 1"
                    notify('Success')
                }
            }
        }




 }
}
def notify(status){
    emailext (
      to: "pratik.patel@motadata.com",
      subject: "${status}: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
      body: """<p>${status}: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]':</p>
        <p>Check console output at <a href='${env.BUILD_URL}'>${env.JOB_NAME} [${env.BUILD_NUMBER}]</a></p>
      <p>Test Case Analysis .<br>
Total Test Cases: 10  <br>
Passed Test Cases: 8  <br>
Failed Test Cases: 2  <br>
Skipped Test Cases: 0  <br>
</p>


<p>Please find Sonar report from below link <br>
172.16.8.233:9000/dashboard/index/com.nms:motadata7 <br></p>
""",
    )

}



