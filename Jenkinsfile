pipeline {
    agent any

    environment {
        ORCHESTRATOR_URL = "https://cloud.uipath.com/cicdcloud"
        TENANT_NAME = "DefaultTenant"
        FOLDER_PATH = "/CI-CD Uipath"
        RELEASE_NAME = "Uipath-Jenkins-CICD"
        PACKAGE_VERSION = "1.15"
        DESCRIPTION = "Deploy test cases to Orchestrator"
        RELEASE_NOTES = "Release notes"
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from the Git repository
                git branch: 'main', url: 'https://github.com/Patrick1411/Uipath-Jenkins-Pipepline.git'
            }
        }

        stage('Build Package') {
            steps {
                // Build the UiPath package
                bat '\"C:\Users\DuyThienNguyen\AppData\Local\Programs\UiPath\Studio\UiRobot.exe" --pack \"C:\\Projects\\CICD-Uipath\\project.json\" --output \"C:\\Projects\\Deploy\\CICD-Uipath.$PACKAGE_VERSION.nupkg"'
            }
        }

        stage('Deploy Package to Orchestrator') {
            steps {
                // Deploy the package to Orchestrator
                script {
                    def response = sh (
                        script: "curl -X POST -H \"Authorization: Bearer $ORCHESTRATOR_TOKEN\" -F \"file=@C:\\Projects\\Deploy\\CICD-Uipath\\CICD-Uipath.${PACKAGE_VERSION}.nupkg" -F \"json={ 'PackageVersion': '$PACKAGE_VERSION', 'Description': '$DESCRIPTION', 'ReleaseNotes': '$RELEASE_NOTES' }\" $ORCHESTRATOR_URL/odata/Processes/UiPath.Server.Configuration.OData.UploadPackage",
                        returnStdout: true
                    )

                    def packageId = response.split(': ')[1].replaceAll('"', '').trim()

                    sh "curl -X POST -H \"Authorization: Bearer $ORCHESTRATOR_TOKEN\" -H \"Content-Type: application/json\" -d '{ \"Package\": { \"Id\": $packageId }, \"Name\": \"$RELEASE_NAME\", \"Description\": \"$DESCRIPTION\", \"ReleaseNotes\": \"$RELEASE_NOTES\", \"ProcessVersion\": \"$PACKAGE_VERSION\" }' $ORCHESTRATOR_URL/odata/Releases/UiPath.Server.Configuration.OData.PublishRelease?tenantName=$TENANT_NAME&folderPath=$FOLDER_PATH"
                }
            }
        }
    }
}
