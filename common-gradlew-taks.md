# Common gradlew taks

`$ ./gradlew tasks`

## Android tasks

androidDependencies - Displays the Android dependencies of the project.

signingReport - Displays the signing info for each variant.

sourceSets - Prints out all the source sets defined in this project.

## Build tasks

assemble - Assembles all variants of all applications and secondary packages.

assembleAndroidTest - Assembles all the Test applications.

assembleDebug - Assembles all Debug builds.

assembleRelease - Assembles all Release builds.

build - Assembles and tests this project.

buildDependents - Assembles and tests this project and all projects that depend on it.

buildNeeded - Assembles and tests this project and all projects it depends on.

compileDebugAndroidTestSources

compileDebugSources

compileDebugUnitTestSources

compileReleaseSources

compileReleaseUnitTestSources

extractDebugAnnotations - Extracts Android annotations for the debug variant into the archive file

extractReleaseAnnotations - Extracts Android annotations for the release variant into the archive file

mockableAndroidJar - Creates a version of android.jar that's suitable for unit tests.

## Build Setup tasks

init - Initializes a new Gradle build. \[incubating\]

wrapper - Generates Gradle wrapper files. \[incubating\]

Help tasks

components - Displays the components produced by root project 'SixtFleetCheck'. \[incubating\]

dependencies - Displays all dependencies declared in root project 'SixtFleetCheck'.

dependencyInsight - Displays the insight into a specific dependency in root project 'SixtFleetCheck'.

help - Displays a help message.

model - Displays the configuration model of root project 'SixtFleetCheck'. \[incubating\]

projects - Displays the sub-projects of root project 'SixtFleetCheck'.

properties - Displays the properties of root project 'SixtFleetCheck'.

tasks - Displays the tasks runnable from root project 'SixtFleetCheck' \(some of the displayed tasks may belong to subprojects\).

## **Install tasks**

installDebug - Installs the Debug build.

installDebugAndroidTest - Installs the android \(on device\) tests for the Debug build.

installRelease - Installs the Release build.

uninstallAll - Uninstall all applications.

uninstallDebug - Uninstalls the Debug build.

uninstallDebugAndroidTest - Uninstalls the android \(on device\) tests for the Debug build.

uninstallRelease - Uninstalls the Release build.

## React tasks

bundleDebugJsAndAssets - bundle JS and assets for Debug.

bundleReleaseJsAndAssets - bundle JS and assets for Release.

## Verification tasks

check - Runs all checks.

clean - Deletes the build directory.

connectedAndroidTest - Installs and runs instrumentation tests for all flavors on connected devices.

connectedCheck - Runs all device checks on currently connected devices.

connectedDebugAndroidTest - Installs and runs the tests for debug on connected devices.

deviceAndroidTest - Installs and runs instrumentation tests using all Device Providers.

deviceCheck - Runs all device checks using Device Providers and Test Servers.

lint - Runs lint on all variants.

lintDebug - Runs lint on the Debug build.

lintRelease - Runs lint on the Release build.

test - Run unit tests for all variants.

testDebugUnitTest - Run unit tests for the debug build.

testReleaseUnitTest - Run unit tests for the release build.

## Other tasks

assembleDefault

copyDownloadableDepsToLibs

jarDebugClasses

jarReleaseClasses

