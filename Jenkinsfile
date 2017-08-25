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
        if (utils.isCI()){
                            def name = "dotnet"
                            def location = "https://raw.githubusercontent.com/redhat-developer/s2i-dotnetcore/master/dotnet_imagestreams.json"
                           if (flow.openShiftImageStreamInstall(name,location)){
                                echo "YAYYYY!!!!!"
                           }
                        }
}
