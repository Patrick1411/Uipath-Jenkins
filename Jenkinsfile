pipeline {
    agent any

    environment {
        // Environment Variables
        MAJOR = '1'
	    MINOR = '1'
        //Orchestrator Services
        UIPATH_ORCH_URL = "https://cloud.uipath.com/cicdcloud"
        UIPATH_ORCH_TENANT_NAME = "DefaultTenant"
        UIPATH_ORCH_FOLDER_NAME = "CI-CD Uipath"
        RELEASE_NAME = "Uipath-Jenkins-CICD"
        PACKAGE_VERSION = "1.15"
    }

    stages {

        // Printing Basic Information:
        stage('Preparing'){
            steps {
                echo "Jenkins Home ${env.JENKINS_HOME}"
                echo "Jenkins URL ${env.JENKINS_URL}"
                echo "Jenkins JOB Number ${env.BUILD_NUMBER}"
                echo "Jenkins JOB Name ${env.JOB_NAME}"
                echo "GitHub BranchName ${env.BRANCH_NAME}"
                checkout scm
            }
        }

        Building Testsr
        stage('Build Tests') {
            steps {
                echo "Building package with ${WORKSPACE}"
                UiPathPack (
                    outputPath: "Output\\${env.BUILD_NUMBER}",
                    outputType: 'Tests',
                    projectJsonPath: "project.json",
                    version: [$class: 'ManualVersionEntry', version: "${MAJOR}.${MINOR}.${env.BUILD_NUMBER}"]
                    useOrchestrator: false,
                    traceLoggingLevel: "None",
                )
            }
        }

        // stage ('Deploy Tests') {
        //     steps {
        //         echo "Deploying ${BRANCH_NAME} to orchestrator"
        //         UiPathDeploy (
        //             packagePath: "path\\to\NuGetpackage",
        //             orchestratorAddress: "${UIPATH_ORCH_URL}",
        //             orchestratorTenant: "${UIPATH_ORCH_TENANT_NAME}",
        //             folderName: "${UIPATH_ORCH_FOLDER_NAME}",
        //             credentials: [$class: 'UserPassAuthenticationEntry', credentialsId: “credentialsId”],
        //             traceLoggingLevel: 'None'
        //         )
        //     }
        // }
    }
}
