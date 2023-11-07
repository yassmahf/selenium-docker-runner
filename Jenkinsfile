pipeline{

agent any


stages{
    stage('Start Grid'){
        steps{
            sh "docker-compose -f grid.yaml up -d"

        }

    }
 
    stage('Run Test'){

        steps{sh "docker-compose -f test-suites.yaml up"
        }

    }



}

post{
always{
    sh "docker-compose -f grid.yaml down"
        sh "docker-compose -f test-suites.yaml down"
  always{archiveArtifacts artifacts: 'output/flight-reservation/emailable-report.html', followSymlinks: false}
                        always{archiveArtifacts artifacts: 'output/vendor-portal/emailable-report.html', followSymlinks: false}

}
        

    }







}