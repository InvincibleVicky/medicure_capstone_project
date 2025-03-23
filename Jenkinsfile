pipeline {
    agent any
    
    stages {
        stage("Git Checkout") {
            steps {
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

         stage("Test the Code") {
            steps {
                echo "Starting Testing"
                sh "mvn test"
            }
        }

        stage("QA of the Code") {
            steps {
                echo "Starting Compilation"
                sh "mvn checkstyle:checkstyle"
            }
        }

         stage("Create a Package") {
            steps {
                echo "Packaging Application"
                sh "mvn package"
            }
        }

        stage('Publish the HTML Reports') {
             steps {
              publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: false, 
                       reportDir: '/var/lib/jenkins/workspace/insure-me/target/surefire-reports',
                       reportFiles: 'index.html', reportName: 'HTML Report', reportTitles: '', 
                       useWrapperFileDirectly: true])
            }
        }

        stage('Create a Docker image from the Package medicure.jar file') {
            steps {
                sh 'docker build -t vigneshwar1908/medicure_healthcare:v1 .'
            }
        }
        
        stage('Login to Dockerhub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-login', passwordVariable: 'dockerpass', usernameVariable: 'dockeruser')]) {
                sh 'docker login -u ${dockeruser} -p ${dockerpass}'                                                       }
            }
        }

        stage('Push the Docker image') {
            steps {
                sh 'docker push vigneshwar1908/medicure_healthcare:v1'
            }
        }

       stage('Deploy to k8s'){
            steps{
                kubernetesDeploy (configs: 'deployment.yml, service.yml' ,kubeconfigId: 'k8sconfigpwd')
            }
        }
  }
}
