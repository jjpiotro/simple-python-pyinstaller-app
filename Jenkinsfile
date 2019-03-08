pipeline{
    agent none
    stages{
        stage("Build"){
            agent {
                docker {
                    image 'python:2-alpine'
                }
            }
            steps{
                echo "====++++executing Build++++===="
            }
            post{
                always{
                    echo "====++++always++++===="
                }
                success{
                    echo "====++++Build executed succesfully++++===="
                }
                failure{
                    echo "====++++Build execution failed++++===="
                }
        
            }
        }
        stage("Test"){
            agent{
                docker{
                    image 'qnib/pytest'
                }
            }
            steps{
                echo "====++++executing Test++++===="
                sh 'py.test --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
            }
            post{
                always{
                    echo "====++++always++++===="
                    junit 'test-reports/results.xml'
                }
                success{
                    echo "====++++Test executed succesfully++++===="
                }
                failure{
                    echo "====++++Test execution failed++++===="
                }
        
            }
        }
        stage("Deliver"){
            agent{
                docker{
                    image 'cdrx/pyinstaller-linux:python2'
                }
            }
            steps{
                echo "====++++executing Deliver++++===="
                sh 'pyinstaller --onefile sources/add2vals'
            }
            post{
                always{
                    echo "====++++always++++===="
                }
                success{
                    echo "====++++Deliver executed succesfully++++===="
                    archiveArtifacts 'dist/add2vals'
                }
                failure{
                    echo "====++++Deliver execution failed++++===="
                }
        
            }
        }
    }
    post{
        always{
            echo "========always========"
        }
        success{
            echo "========pipeline executed successfully ========"
        }
        failure{
            echo "========pipeline execution failed========"
        }
    }
}