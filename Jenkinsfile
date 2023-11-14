pipeline{

    agent any
 parameters {
  choice choices: ['chrome', 'firefox'], description: 'Select the browser', name: 'BROWSER'
}
    stages{

        stage('Start Grid'){
            steps{
                sh "docker-compose -f grid.yaml --scale ${params.BROWSER}=1 up -d"
            }
        }

        stage('Run Test vendor-portal'){
            steps{
                sh "docker-compose -f test-suites.yaml TEST-SUITE=vendor-portal up --pull=always"
                script{
                    if(fileExists('output/flight-reservation/testng-failed.xml')||
                    fileExists('output/vendor-portal/testng-failed.xml')){
                        error('failed tests found')
                    }

                }
            }
        }

        







    }

    post {
        always {
            sh "docker-compose -f grid.yaml down"
            sh "docker-compose -f test-suites.yaml down"
            archiveArtifacts artifacts: 'output/flight-reservation/emailable-report.html', followSymlinks: false
            archiveArtifacts artifacts: 'output/vendor-portal/emailable-report.html', followSymlinks: false
        }
    }

}