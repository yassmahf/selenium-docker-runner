pipeline{

    agent any
 parameters {
  choice choices: ['chrome', 'firefox'], description: 'Select the browser', name: 'BROWSER'
}
    stages{

        stage('Start Grid'){
            steps{
                sh "docker-compose -f grid.yaml up --scale ${params.BROWSER}=1 -d"
            }
        }

        stage('Run Test vendor-portal'){
            steps{
                sh "TEST_SUITE=vendor-portal.xml TEST_OUTPUT_VOLUME=./output/vendor-portal docker-compose -f test-suites.yaml up --pull=always"
                script{
                    if(
             fileExists('output/vendor-portal/testng-failed.xml')){
                        error('failed tests found')
                    }

                }
            }
        }

    stage('Run Test flight-reservation'){
            steps{
                sh "TEST_SUITE=flight-reservation.xml TEST_OUTPUT_VOLUME=./output/flight-reservation docker-compose -f test-suites.yaml up --pull=always"
                script{
                    if(fileExists('output/flight-reservation/testng-failed.xml')){
                        error('failed tests found')
                    }

                }
            }
        }







    }

    post {
        always {
            sh "docker-compose -f grid.yaml down"
           // sh "docker-compose -f test-suites.yaml down"
                       sh "TEST_OUTPUT_VOLUME=./output/flight-reservation docker-compose -f test-suites.yaml down"
            sh "TEST_OUTPUT_VOLUME=./output/vendor-portal docker-compose -f test-suites.yaml down"
            archiveArtifacts artifacts: 'output/flight-reservation/emailable-report.html', followSymlinks: false
            archiveArtifacts artifacts: 'output/vendor-portal/emailable-report.html', followSymlinks: false
        }
    }

}