@Library('jenkins-shared-library') _
def configMap = [
    project : "roboshop",
    component : "catalouge"
]
if (env.BRANCH_NAME.equalsIgnoreCase('main')){
    echo "We will deal later"
}
else {
    nodejsEKSPipeline(configMap)
}
//testpipeline(configMap)