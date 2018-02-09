#!groovy

node {
    checkout scm
    openshift.withProject("getup-images") {
        createBuildConfig()
        build()
    }
}

def createBuildConfig()
{
    def bc = openshift.selector("buildconfig", "${JOB_NAME}")

    if (!bc.exists()) {
        print "Creating BuildConfig ${JOB_NAME}"
        def objects = openshift.process(readFile(file: 'template.yaml'),
                            "-p=JOB_NAME=${JOB_NAME}",
                            "-p=SOURCE_GIT_URI=https://github.com/getupcloud/backup.git",
                            "-p=OUTPUT_IMAGE=docker.io/getupcloud/${JOB_NAME}:latest"
                            )
        openshift.create(objects)
    } else {
        print "BuildConfig ${JOB_NAME} already exists"
    }
}

def build()
{
    openshiftBuild(buildConfig: "${JOB_NAME}")
}
