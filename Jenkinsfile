node {

   stage('Preparation') { // for display purposes
      // Get some code from a GitHub repository
        checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, 
        extensions: [[$class: 'WipeWorkspace']], submoduleCfg: [],
        userRemoteConfigs: [[credentialsId: '497e439b-9460-4ed0-a22a-ded60c5634c4', url: 'https://github.com/InckyIncky/OtusCourse.POM.git']]])      // Get the Maven tool.
      
      // ** NOTE: This 'M3' Maven tool must be configured
      // **       in the global configuration.           
      //mvnHome = tool 'M3'
   }
   stage('Build') {
       
    withMaven(maven: 'maven 3.6.3') {
    // some block
    bat label: '', script: 'mvn clean test'
    }
      // Run the maven build

        //  if (isUnix()) {
        //     sh '"$MVN_HOME/bin/mvn" -Dmaven.test.failure.ignore clean package'
        //  } else {
        //     bat label: '', script: 'mvn clean test'
        //  }
   }
   stage('Results') {
      junit '**/target/surefire-reports/TEST-*.xml'
    //   archiveArtifacts 'target/*.jar'
      
      allure jdk: '', results: [[path: 'target/allure-results']]
      
      emailext body: '''Testname: $PROJECT_NAME 
                Build number: $BUILD_NUMBER 
                Status : $BUILD_STATUS
                
                Test results:
                Total: ${TEST_COUNTS,var="total"} 
                Passed: ${TEST_COUNTS,var="pass"}
                Failed: ${TEST_COUNTS,var="fail"} 
                
                Failed tests :  ${FAILED_TESTS} 
                
                Check console output at $BUILD_URL to view the results.''', subject: '$DEFAULT_SUBJECT', to: 'inkckyincky@yandex.ru, inckyincky049@gmail.com'
   }
}
