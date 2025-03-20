pipeline{
  agent any
  
    stages{
      
      stage("Git Checkout"){
        steps{
          echo "Cloning Git Repo"
          git url: "https://github.com/InvincibleVicky/medicure_capstone_project/"
        }
      }

      stage("Compile the Code") {
            steps {
                echo "Starting Compilation"
                sh "mvn compile"
            }
        }
    
  }
}
