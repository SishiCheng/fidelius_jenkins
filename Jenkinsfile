pipeline {
      agent {node {label 'SGX20'}}

      stages {
        stage('Clone') {
          steps {
            sh '''#bin/bash
                export http_proxy=http://10.10.10.10:8118 https_proxy=http://10.10.10.10:8118
                #rm -rf YeeZ-Privacy-Computing
                if [ ! -d YeeZ-Privacy-Computing ]; then
                  git clone https://github.com/YeeZTech/YeeZ-Privacy-Computing.git
                  cd YeeZ-Privacy-Computing
                  git checkout develop
                else
                  cd YeeZ-Privacy-Computing
                  git pull origin develop
                fi

                git submodule update --init
              '''
          }
        }

        stage("Build"){
          steps{
            sh '''#bin/bash
              cd YeeZ-Privacy-Computing
              rm -rf build_debug
              cmake -DSGX_MODE=Debug -S . -B ./build_debug
              cmake --build ./build_debug
            '''
          }
        }

        stage("Test HPDA"){
          steps{
            catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
            sh '''#bin/bash
              cd YeeZ-Privacy-Computing/bin
              ./hpda_gtest
            '''
          }
          }
        }

        stage("Test Core"){
          steps{
            catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
            sh '''#bin/bash
              cd YeeZ-Privacy-Computing/bin
              echo "athrun911500" | ./gtest_core
            '''
          }
        }
        }

        stage("Test Crypto"){
          steps{
            catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
            sh '''#bin/bash
              cd YeeZ-Privacy-Computing/bin
              ./test_crypto
            '''
          }
        }
        }

        stage("Test Iris"){
          steps{
            catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
            sh '''#bin/bash
              cd YeeZ-Privacy-Computing
              python3 test/integrate/test_iris.py
            '''
          }
        }
        }

        stage("Test Find Person"){
          steps{
            catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
            sh '''#bin/bash
              cd YeeZ-Privacy-Computing
              python3 test/integrate/test_findperson.py
            '''
          }
        }
        }
      }// end stages

}

