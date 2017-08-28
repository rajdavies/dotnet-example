#!/usr/bin/groovy

@Library('github.com/rajdavies/fabric8-pipeline-library@dev11')

def localItestPattern = ""
try {
  localItestPattern = ITEST_PATTERN
} catch (Throwable e) {
  localItestPattern = "*IT"
}

def localFailIfNoTests = ""
try {
  localFailIfNoTests = ITEST_FAIL_IF_NO_TEST
} catch (Throwable e) {
  localFailIfNoTests = "false"
}

def versionPrefix = ""
try {
  versionPrefix = VERSION_PREFIX
} catch (Throwable e) {
  versionPrefix = "1.0"
}

def canaryVersion = "${versionPrefix}.${env.BUILD_NUMBER}"

def fabric8Console = "${env.FABRIC8_CONSOLE ?: ''}"
def utils = new io.fabric8.Utils()
def label = "buildpod.${env.JOB_NAME}.${env.BUILD_NUMBER}".replace('-', '_').replace('/', '_')
def envStage = utils.environmentNamespace('stage')
def envProd = utils.environmentNamespace('run')
def stashName = ""
def deploy = false
node {
  checkout scm

  def name = "dotnet"
          def location = "https://raw.githubusercontent.com/redhat-developer/s2i-dotnetcore/master/dotnet_imagestreams.json"
          if (flow.openShiftImageStreamInstall(name,location)){
                                  echo "YAYYYY!!!!!"
          }

  if (utils.isCD()){
    deploy = true
    echo 'NOTE: running pipelines for the first time will take longer as build and base docker images are pulled onto the node'
    container(name: 'maven') {

      stage('Build Release'){
        canaryRelease {
          version = canaryVersion
        }
      }

      stage('Integration Testing'){

      }

      stage('Rollout to Stage'){
        kubernetesApply(environment: envStage)
        //stash deployments
        stashName = label
        stash includes: '**/*.yml', name: stashName
      }
    }
  }
}

if (deploy){
    node {
        stage('Approve'){
          approve {
            room = null
            version = canaryVersion
            console = fabric8Console
            environment = 'Stage'
          }
        }

        stage('Rollout to Run'){
          unstash stashName
          kubernetesApply(environment: envProd)
        }
    }
}

