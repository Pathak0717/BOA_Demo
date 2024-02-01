pipeline {
    agent any
 
    stages {
        stage('Download and Install Application') {
            steps {
                script {
                    // Define application release URL
                    def applicationUrl = 'https://github.com/GoogleCloudPlatform/bank-of-anthos/releases/tag/v0.6.2'
 
                    // Download and extract the application
                    sh "wget ${applicationUrl} -O bank-of-anthos-0.6.2.zip"
                    sh 'unzip bank-of-anthos.zip -d bank-of-anthos-0.6.2'
 
                    // Clean up the zip file
                    sh 'rm bank-of-anthos-0.6.2.zip'
                }
            }
        }
 
        stage('Install YAML') {
            steps {
                script {
                    // Install YAML (Replace this command with the actual YAML installation command)
                //   sh 'sudo apt-get update && sudo apt-get install -y yaml-package'
						kubectl apply -f ./extras/jwt/jwt-secret.yaml
						kubectl apply -f ./kubernetes-manifests
                    // Install YAML
                   // sh yamlInstallationCommand
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
