# gradle
build gradle properties files templates

Upload module libaray to maven.....

At the end of module's build.gradle, add 

    https://raw.githubusercontent.com/Shuaigoal/gradle/configs/upload-source-aar.gradle

if necessary, add below

    tasks.withType(Javadoc) {
        options.encoding = "UTF-8"
    }

