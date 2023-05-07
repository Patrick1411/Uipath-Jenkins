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
                    //credentials: UserPass(credentialsId: 'UipathCredentials'), 
                    //credentials: Token(accountName: "${UIPATH_ORCH_LOGICAL_NAME}", credentialsId: 'APIUserKey'),
                    ExternalApp: ExternalApp(
                        accountForApp: "${UIPATH_ORCH_LOGICAL_NAME}",
                        applicationId: "24b90442-c557-47c2-9ba3-1ae804c0954c",
                        applicationSecret: "X0A@Ra4Qc9lmbef6",
                        applicationScope: "OR.Administration OR.Administration.Read OR.Administration.Write OR.Analytics OR.Analytics.Read OR.Analytics.Write OR.Assets OR.Assets.Read OR.Assets.Write OR.Audit OR.Audit.Read OR.Audit.Write OR.BackgroundTasks OR.BackgroundTasks.Read OR.BackgroundTasks.Write OR.Execution OR.Execution.Read OR.Execution.Write OR.Folders OR.Folders.Read OR.Folders.Write OR.Hypervisor OR.Hypervisor.Read OR.Hypervisor.Write OR.Jobs OR.Jobs.Read OR.Jobs.Write OR.License OR.License.Read OR.License.Write OR.Machines OR.Machines.Read OR.Machines.Write OR.ML OR.ML.Read OR.ML.Write OR.Monitoring OR.Monitoring.Read OR.Monitoring.Write OR.Queues OR.Queues.Read OR.Queues.Write OR.Robots OR.Robots.Read OR.Robots.Write OR.Settings OR.Settings.Read OR.Settings.Write OR.Tasks OR.Tasks.Read OR.Tasks.Write OR.TestDataQueues OR.TestDataQueues.Read OR.TestDataQueues.Write OR.TestSetExecutions OR.TestSetExecutions.Read OR.TestSetExecutions.Write OR.TestSets OR.TestSets.Read OR.TestSets.Write OR.TestSetSchedules OR.TestSetSchedules.Read OR.TestSetSchedules.Write OR.Users OR.Users.Read OR.Users.Write OR.Webhooks OR.Webhooks.Read OR.Webhooks.Write",
                        identityUrl: "${UIPATH_ORCH_URL}/identity"
                    ),
                    traceLevel: 'None',
                    entryPointPaths: 'Main.xaml',
                    createProcess: false
                )
            }
        }
    }
}
