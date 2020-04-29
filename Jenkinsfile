#!/usr/bin/env groovy

pipeline {
    agent any
	    environment {
        	LC_ALL = 'en_US.UTF-8'
        	LANG    = 'en_US.UTF-8'
        	LANGUAGE = 'en_US.UTF-8'
        }
		options {
    		skipDefaultCheckout(true)
		}
	stages {
		// << Git SCM Checkout >>
		stage('Git Checkout') {
			steps {
				checkout scm
			}
		}

		// // << Copying Provision Profiles to build server>>
		// stage('Provision Profiles')  {
		// 	steps {
		// 		sh 'cp -R Devops/MobileDevice ~/Library/'
		// 	}
		// }

        stage('SwiftFormat') {
		    steps {
				script {
					try {
						swiftformat_cmd = "Pods/SwiftFormat/CommandLineTool/swiftformat ./ --exclude Pods --swiftversion 5.0.1 --wraparguments before-first --wrapcollections before-first --importgrouping testable-bottom"
						sh swiftformat_cmd + " --lint"
						sh "echo SwiftFormat completed successfully, please refer to console output for more info. > build/swiftformat.txt"
					}catch(Exception e) {
						sh "echo Please run Hero unit tests in Xcode with Command+U. This will automatically run swiftformat. > build/swiftformat.txt"
						sh "echo You should ALWAYS run unit tests before submitting a PR. >> build/swiftformat.txt"
						sh "echo Here are the possible chances will be applied, please pay attention to your coding style: >> build/swiftformat.txt"
						sh swiftformat_cmd
					}
				}
			}
  		}

		// << CodeBase formating using SwiftFormat  >>
		// // << CodeBase Linting using SwiftLint  >>
		stage('SwiftLint') {
			steps {
				script {
						try {
							sh "fastlane lint"
						} catch(Exception e){
							currentBuild.result = "UNSTABLE"
						}
			        }
		    }
            post {
                always {
                    step([$class: 'CheckStylePublisher', canComputeNew: false, defaultEncoding: '', healthy: '', pattern: '**/build/swiftlint.result.xml', unHealthy: ''])
                }
            }
		}

		// << CodeBase Unit Testing  >>
		stage('Unit Testing') {
		    steps {
				script {
						try {
                            sh 'pod install'
							sh "fastlane tests --env config"
						} catch(Exception e) {
							currentBuild.result = "UNSTABLE"
							sh "mkdir -p build || true"
						}
				}
			}
			post {
				always {
					junit(testResults: '**/build/report.junit', allowEmptyResults: true)
				}
			}
		}

		// << Swift Unit TestCoverage >>
		stage('TestCoverage') {
			steps {
				script {
						try{
							sh "fastlane analysis --env config"
						}
						catch(Exception e) {
							currentBuild.result = "UNSTABLE"
						}
				}
			}
			post {
				always {
					step([$class: 'CoberturaPublisher', coberturaReportFile: '**/build/cobertura.xml', autoUpdateHealth: false, autoUpdateStability: false,failUnhealthy: false, failUnstable: false, maxNumberOfBuilds: 0, onlyStable: false, sourceEncoding: 'ASCII', zoomCoverageChart: false])
				}
      		}
		}

        // << IOS Build >>
		/*
		stage('IOSBuild') {
			steps {
				script {
						try {
							sh "fastlane env"
							sh "fastlane build --env config"
							sh "echo Build completed successfully, please refer console output for more info.. >> build/result.txt"
						}
						catch(Exception e) {
							currentBuild.result = "UNSTABLE"
							buildresult=false
							sh "echo Build failed, please refer console output for more info.. >> build/result.txt"
						}
				}
		    }
		}
		*/

        // << IOS TestFairyUpload >>
		// stage('TestFairyUpload') {
		// 	steps {
		// 			sh "fastlane upload --env config"
		//     }
		// }
	}
    post {
			always {
				archiveArtifacts artifacts: '**/build/*', onlyIfSuccessful: true
			}
	}
}
