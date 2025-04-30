pipeline {
    agent any

    options {
        skipStagesAfterUnstable()
    }

    stage('Setup') {
        steps {
            sh '''
                apt-get update && \
                apt-get install -y --no-install-recommends python3 python3-pip python3-venv binutils && \
                python3 -m venv venv && \
                . venv/bin/activate && \
                pip install --upgrade pip && \
                pip install -r requirements.txt
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
