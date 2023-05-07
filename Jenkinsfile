pipeline {
    agent any

    environment {
        // Environment Variables
        MAJOR = '1'
	    MINOR = '5'
        //Orchestrator Services
        UIPATH_ORCH_URL = "https://cloud.uipath.com/cicdcloud/DefaultTenant/orchestrator_"
        UIPATH_ORCH_TENANT_NAME = "DefaultTenant"
        UIPATH_ORCH_FOLDER_NAME = "CI-CD Uipath"
        UIPATH_ORCH_LOGICAL_NAME = "Truong"
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

        //Building Tests
        stage('Build Tests') {
            steps {
                echo "Building package with ${WORKSPACE}"
                UiPathPack (
                    outputPath: "Output\\${env.BUILD_NUMBER}",
                    projectJsonPath: "project.json",
                    version: [$class: 'ManualVersionEntry', version: "${MAJOR}.${MINOR}.${env.BUILD_NUMBER}"],
                    useOrchestrator: false,
                    traceLevel: "None"
                )
            }
        }

        // Deploy Stages
        stage ('Deploy Tests') {
            steps {
                echo "Deploying ${env.BRANCH_NAME} to orchestrator"
                UiPathDeploy (
                    packagePath: "Output\\${env.BUILD_NUMBER}",
                    orchestratorAddress: "${UIPATH_ORCH_URL}",
			        orchestratorTenant: "${UIPATH_ORCH_TENANT_NAME}",
			        folderName: "${UIPATH_ORCH_FOLDER_NAME}",
                    environments: "Dev",
                    credentials: [$class: 'UserPassAuthenticationEntry', credentialsId: "APIUserKey"],
                    //credentials: UserPass(credentialsId: 'UipathCredentials'), 
                    //credentials: Token(accountName: "${UIPATH_ORCH_LOGICAL_NAME}", credentialsId: 'APIUserKey'),
                    traceLevel: 'None',
                    entryPointPaths: 'Main.xaml',
                    createProcess: false
                )
            }
        }
    }
}
