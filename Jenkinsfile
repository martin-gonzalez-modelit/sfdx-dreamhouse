#!groovy
import groovy.json.JsonSlurperClassic
node {

    def BUILD_NUMBER=env.BUILD_NUMBER
    def RUN_ARTIFACT_DIR="tests/${BUILD_NUMBER}"
    def SFDC_USERNAME

    def QA_ORG=env.QA_ORG_DH
    def QA_SFDC_HOST = env.QA_SFDC_HOST_DH
    def QA_JWT_KEY_CRED_ID = env.QA_JWT_CRED_ID_DH
    def QA_CONNECTED_APP_CONSUMER_KEY=env.QA_CONNECTED_APP_CONSUMER_KEY_DH
    println 'KEY IS' 
    println JWT_KEY_CRED_ID
    def toolbelt = tool 'toolbelt'

    stage('checkout source') {
        // when running in multi-branch job, one must issue this command
        checkout scm
    }

    withCredentials([file(credentialsId: JWT_KEY_CRED_ID, variable: 'jwt_key_file')]) {
        stage('Connect to QA') {
            if (isUnix()) {
                rc = sh returnStatus: true, script: "${toolbelt} force:auth:jwt:grant --clientid ${CONNECTED_APP_CONSUMER_KEY} --username ${HUB_ORG} --jwtkeyfile ${jwt_key_file} --setalias qa --instanceurl ${SFDC_HOST}"
            }else{
                 rc = bat returnStatus: true, script: "\"${toolbelt}\" force:auth:jwt:grant --clientid ${CONNECTED_APP_CONSUMER_KEY} --username ${HUB_ORG} --jwtkeyfile \"${jwt_key_file}\" --setalias qa --instanceurl ${SFDC_HOST}"
            }
            if (rc != 0) { error 'Connection failed' }
            
        }
        
          stage('Deploy to QA') {
              if (isUnix()) {
                    rc = sh returnStatus: true, script: "\"${toolbelt}\" force:source:deploy -p c:\Users\Pablo\Workspaces\Sfdx\PG&E Dev3\force-app\main -u qa -l RunLocalTests -w -1"
              }else{
                  rc = bat returnStatus: true, script: "\"${toolbelt}\" force:source:deploy -p c:\Users\Pablo\Workspaces\Sfdx\PG&E Dev3\force-app\main -u qa -l RunLocalTests -w -1"
              }
            if (rc != 0) {
                error 'Deploy failed'
            }
            
          }
             
    }
} 
