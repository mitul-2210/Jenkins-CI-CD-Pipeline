pipeline {
    agent any

    options {
        skipStagesAfterUnstable()
    }

    stages {
        stage('Setup') {
            steps {
               sh '''
                    apt-get update && \
                    apt-get install -y --no-install-recommends python3 python3-pip binutils && \
                    python3 -m pip install --upgrade pip && \
                    pip3 install -r requirements.txt
                '''
            }
        }

        stage('Build') {
            steps {
                script {
                    sh 'python3 -m py_compile sources/add2vals.py sources/calc.py'
                    stash name: 'sources', includes: 'sources/**'
                }
            }
        }

        stage('Test') {
            steps {
                sh 'pytest --junit-xml=test-reports/results.xml sources/test_calc.py'
                junit 'test-reports/results.xml'
            }
        }

        stage('Deliver') {
            steps {
                sh 'pyinstaller --onefile sources/add2vals.py'
            }
            post {
                success {
                    archiveArtifacts 'dist/add2vals'
                }
            }
        }
    }
}
