pipeline {
    agent any

    parameters {
        string(name: 'DEST_PATH', defaultValue: '/path/to/destination', description: 'Enter the destination path to copy the compressed file')
        booleanParam(name: 'COPY_TO_DEST', defaultValue: false, description: 'Select Yes to copy the compressed file to the destination path')


    }

    environment {
        // Define variables for reuse--
        GIT_REPO = 'https://github.com/your-username/your-repo.git' // Replace with your GitHub repo URL
        BRANCH = 'master' // Replace with your target branch
        //ZIP_FILE = 'html_files.zip'
       //TAR_FILE = 'html_files.tar.gz' 
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from GitHub
                //git branch: "${BRANCH}", url: "${GIT_REPO}"
                git credentialsId: 'd466d8e7-e5ba-4168-9c64-549e475bcbea', url: 'https://github.com/Knoweno/GCP-TEST.git'
            }
        }

        stage('Zip  Files 2') {
            steps {
                script {
				 // Get the build number
                    def buildNumber = env.BUILD_NUMBER
                    // Get the pipeline name or title
                    def pipelineName = env.JOB_NAME.replaceAll('/', '-')
                    //def pipelineName = env.BUILD_DISPLAY_NAME ? env.BUILD_DISPLAY_NAME : 'default'
                    // Create the tar file name using the branch name, pipeline name, and build number
                    env.TAR_FILE = "${BRANCH}-${pipelineName}-${buildNumber}.tar.gz"
				
                    
                    // Debug: Print the tar file name
                    echo "Creating tar file: ${env.TAR_FILE}"
                    
                    // Compress the HTML files using the generated file name
                    sh "tar -czf '${env.TAR_FILE}' *.html"  // Use quotes to ensure correct handling of spaces
               
                }
            }
        }

        stage('Archive Artifacts') {
            steps {
                archiveArtifacts artifacts: "${env.TAR_FILE}", allowEmptyArchive: false
            }
        }
         /*stage('Copy to Destination') {
            steps {
                script {
                    // Copy the compressed file to the destination path provided as a parameter
                    sh "cp ${TAR_FILE} ${params.DEST_PATH}"
                }
            }*/
        stage('Copy to Destination') {
            when {
                expression { return params.COPY_TO_DEST == true }  // Run only if the user selects Yes
            }
            steps {
                script {
                    // Copy the compressed file to the destination path provided as a parameter
                   // sh "cp ${env.TAR_FILE} ${params.DEST_PATH}"
                    sh "cp '${env.TAR_FILE}' '${params.DEST_PATH}'"
                }
            }
        }
    stage('Print Current Working Directory') {
            steps {
                script {
                    // Print the current working directory
                    def cwd = sh(script: 'pwd', returnStdout: true).trim()
                    echo "Current Working Directory: ${cwd}"
                }
            }
        }
         /*stage('Cleanup Workspace') {
            steps {
                // Clean up the workspace to remove all files created during the build
                cleanWs()
            }
        }*/
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Please check the logs.'
        }
    }
}
