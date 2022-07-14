pipeline {
    agent none
    stages {
        stage('Build') {
            agent {
                docker {
                    image 'python:3.8-alpine'
                }
            }
            steps {
                sh 'python3.8 -m py_compile sources/prog.py sources/calc.py'
                stash(name: 'compiled-results', includes: 'sources/*.py*')
                bash 'echo -e "\n\n Hello World! This is the BUILD stage \n\n"'
            }
        }
    stage('Test') {
            agent {
                docker {
                    image 'grihabor/pytest'
                }
            }
            steps {
                sh 'pyvtest -v --junit-xml test-reports/results.xml sources/test_calc.py'
                bash 'echo -e "\n\n Hello World! This is the TEST stage \n\n"'
            }
            post {
                always {
                    junit "test-reports/results.xml"
                }
            }
        }
    }
}
