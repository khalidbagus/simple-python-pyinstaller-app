node {
    // Mendifinisikan agen Docker
    docker.image('python:2-alpine').inside('-u root') {
        checkout scm

        // Konversi Build Stage
        stage('Build') {
            try {
                sh 'python -m py_compile sources/add2vals.py sources/calc.py'
            } catch (Exception e) {
                currentBuild.result = 'FAILURE'
                throw e
            }
        }

        // Konversi Test Stage
        stage('Test') {
            try {
                sh 'pip install pytest'
                sh 'pytest --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
                junit 'test-reports/results.xml'
            } catch (Exception e) {
                currentBuild.result = 'FAILURE'
                throw e
            }
        }

        // Konversi Deliver Stage
        stage('Deliver') {
            try {
                sh 'pip install pyinstaller'
                sh 'pyinstaller --onefile sources/add2vals.py'
                archiveArtifacts artifacts: 'dist/add2vals', allowEmptyArchive: true
            } catch (Exception e) {
                currentBuild.result = 'FAILURE'
                throw e
            }
        }
    }
}
