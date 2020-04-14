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


		// << Pods Integrity Stage >>
		// stage('PodsIntegrity') {
		// 	steps {
		// 		script {
		// 			try {
		// 				sh "git branch -a"
		// 				sh "./DevOps/ci/script/check-pods-integrity.swift > " + s.resultsOutputFile
		// 				// sh "./DevOps/ci/script/check-pods-integrity > " + s.resultsOutputFile
		// 			} catch(Exception e) {
		// 				currentBuild.result = "UNSTABLE"
		// 				s.failed = true
		// 				sh "echo PodsIntegrity stage failed. >> " + s.resultsOutputFile
		// 			}
		// 			stash includes: s.resultsOutputFile, name: s.stashName
		// 		}
		// 	}
		// }

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
			// post {
			// 	always {
			// 		// step([$class: 'CheckStylePublisher', canComputeNew: false, defaultEncoding: '', healthy: '', pattern: '**/build/swiftlint.result.xml', unHealthy: ''])
			// 		// stash includes: s.resultsOutputFile, name: s.stashName
			// 	}
			// }
		}

		// << CodeBase Unit Testing  >>
		stage('Unit Testing') {
		    steps {
				script {

						try {
							sh "fastlane tests --env config"
						} catch(Exception e) {
							currentBuild.result = "UNSTABLE"
							sh "mkdir -p build || true"
						}
				}
			}
			// post {
			// 	always {
			// 		junit(testResults: '**/build/report.junit', allowEmptyResults: true)
			// 	}
			// }
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
			// post {
			// 	success {
			// 		step([$class: 'CoberturaPublisher', coberturaReportFile: '**/build/cobertura.xml', autoUpdateHealth: false, autoUpdateStability: false,failUnhealthy: false, failUnstable: false, maxNumberOfBuilds: 0, onlyStable: false, sourceEncoding: 'ASCII', zoomCoverageChart: false])
			// 	}
      		// }
		}

	}
}
