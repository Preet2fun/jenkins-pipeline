pipeline{

  agent any
  stages{
    stage('Create build output and archive it'){
      steps{
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
 }
}


