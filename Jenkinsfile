#!/usr/bin/groovy
@Library('github.com/rajdavies/fabric8-pipeline-library@dev11')
def utils = new io.fabric8.Utils()
def flow = new io.fabric8.Fabric8Commands()
def project = 'fabric8-ui/fabric8-ui'
def ciDeploy = false
def imageName
node{
    properties([
        disableConcurrentBuilds()
        ])
}

    dockerNode{
        timeout(time: 1, unit: 'HOURS') {
            ws {
                checkout scm
                readTrusted 'release.groovy'
                def pipeline = load 'release.groovy'

                if (utils.isCI()){

                   if (flow.openShiftImageStreamInstall("dotnet", "https://raw.githubusercontent.com/redhat-developer/s2i-dotnetcore/master/dotnet_imagestreams.json
")){
                        echo "YAYYYY!!!!!"
                   }
                }
            }
        }
    }
