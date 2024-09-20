pipeline {
    agent any

    environment {
        // Define variables for reuse
        GIT_REPO = 'https://github.com/your-username/your-repo.git' // Replace with your GitHub repo URL
        BRANCH = 'master' // Replace with your target branch
        //ZIP_FILE = 'html_files.zip'
        TAR_FILE = 'html_files.tar.gz' 
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
                    // Zip all the HTML files into one archive
                    //sh "zip -r ${ZIP_FILE} *.html"
                     sh "tar -czf ${TAR_FILE} *.html"
                    //sh 'zip -r html_files.zip *.html'
                }
            }
        }

        stage('Archive Artifacts') {
            steps {
                // Archive the ZIP file as a build artifact
                //archiveArtifacts artifacts: "${ZIP_FILE}", fingerprint: true
                 archiveArtifacts artifacts: "${TAR_FILE}", allowEmptyArchive: false
                //archiveArtifacts artifacts: 'html_files.zip', followSymlinks: false
            }
        }
         stage('Cleanup Workspace') {
            steps {
                // Clean up the workspace to remove all files created during the build
                cleanWs()
            }
        }
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
