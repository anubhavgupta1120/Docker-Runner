pipeline {
    agent any
    parameters {
        choice(name: 'Browser', choices: ['Chrome', 'Firefox'], description: 'Select the Browser')
        choice(name: 'TestSuite', choices: ['FlightBooking', 'VendorPortal', 'QAEnv'], description: 'Select the Test-Suite')
    }
    stages {
        stage('Start Grid') {
            steps {
                sh "docker-compose -f Grid.yaml up -d"
            }
        }
        stage('Run Test-Suite') {
            steps {
                sh "BROWSER=$params.Browser TEST_SUITE=$params.TestSuite.xml docker-compose -f Test-Suite.yaml up"
            }
        }
    }
    post {
        always {
            sh "docker-compose -f Test-Suite.yaml down"
            sh "docker-compose -f Grid.yaml down"
            // archiveArtifacts artifacts: "Docker-Output/${params.Test-Suite}/${params.Browser}/TestReport.html", followSymlinks: false
        }
        cleanup {
        /* clean up tmp directory */
            dir("${workspace}@tmp") {
                 deleteDir()
            }
        /* clean up script directory */
            dir("${workspace}@script") {
                 deleteDir()
            }
        }
    }
    
}