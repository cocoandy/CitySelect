# CitySelect
仿照美团地址选择器


# 依赖库的build.gradle
def SDK_BASENAME = "TestSdk";
def SDK_VERSION = "_V1.0";
def sdkDestinationPath = "build";
def zipFile = file('build/intermediates/bundles/default/classes.jar')

task deleteBuild(type: Delete) {
    delete sdkDestinationPath + SDK_BASENAME + SDK_VERSION + ".jar"
}

task makeJar(type: Jar) {
    from zipTree(zipFile)
    from fileTree(dir: 'src/main',includes: ['assets/**'])//将assets目录打入jar包
    baseName = SDK_BASENAME + SDK_VERSION
    destinationDir = file(sdkDestinationPath)
}

makeJar.dependsOn(deleteBuild, build)

