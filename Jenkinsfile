pipeline {
    agent any
 
    environment {
        // Define the GitHub release URL and application version
        GITHUB_RELEASE_URL = 'https://github.com/GoogleCloudPlatform/bank-of-anthos/releases/tag/v0.6.2'
        APP_VERSION = 'v0.6.2'
 
        // Define the Kubernetes cluster name
        KUBE_CLUSTER_NAME = 'prodBOA'
    }
 
    stages {
        stage('Download and Install App') {
            steps {
                script {
                    // Create a directory for the app
                    def appDir = "${WORKSPACE}/bank-of-anthos"
                    dir(appDir) {
                        // Download the app from GitHub release
                        //sh "curl -LJO ${GITHUB_RELEASE_URL}.zip"
			    sh "curl -LJO bank-of-anthos.zip ${GITHUB_RELEASE_URL}.zip"
 
                        // Extract the downloaded app (assuming it's a zip or tar file)
                        sh "unzip ${APP_VERSION}"  // Update this line if the app is in a different format
 
                        // Install the YAML file to the Kubernetes cluster
                        dir("${appDir}/k8s") {
                            // Assuming kubectl is configured in your Jenkins environment
                           // sh "kubectl apply -f your-app-deployment.yaml"
						   sh 'kubectl apply -f ./extras/jwt/jwt-secret.yaml'
						   sh 'kubectl apply -f ./kubernetes-manifests'
                        }
                    }
                }
            }
        }
 
          stage('Execute JMeter Script') {
            steps {
                script {
                    // Replace 'your_jmeter_script_path' with the actual path to your JMeter script
                  //  def jmeterScriptPath = 'your_jmeter_script_path'
				  // Set the path to your JMeter installation
				def JMETER_HOME='/home/ec2-user/jmeter/apache-jmeter-5.6/bin'
 
				// Set the path to your JMeter test file (.jmx)
				def JMETER_TEST_FILE='/home/ec2-user/jmeter/apache-jmeter-5.6/bin/Demo_BOA_Payment_Deposit.jmx'
 
				// Set additional JMeter options if needed
				def JMETER_OPTIONS='-n -t'
                    // Execute the JMeter script
                    sh "${JMETER_HOME}/jmeter ${JMETER_OPTIONS} ${JMETER_TEST_FILE} -l ${JMETER_HOME}/result3.jtl -JThreads=$Concurrent_user -JRampup=$Ramp_up -JDuration=$Duration -Jjmeter.save.saveservice.output_format=xml"
                }
            }
        }
    }

post {
        success {
            echo 'Pipeline succeeded.'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}
