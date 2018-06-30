# gradle

You should add needed keys to properties file in project or module

    build.properties

Modify the module build.gradle's content to...


    if(isModule.toBoolean()) {
        apply plugin: 'com.android.application'
    }else{
        apply plugin: 'com.android.library'
    }

    apply plugin: 'kotlin-android'
    apply plugin: 'kotlin-android-extensions'
    android {

        compileSdkVersion build_versions.compileSdkVersion//27
        defaultConfig {
            if(isModule.toBoolean()) {
                applicationId "com.icarbox.demo.modules"
            }
            minSdkVersion build_versions.minSdkVersion//18
            targetSdkVersion build_versions.targetSdkVersion//27
            versionCode 1
            versionName "1.0"
            testInstrumentationRunner "android.support.test.runner.AndroidJUnitRunner"

            javaCompileOptions {
                annotationProcessorOptions {
                    arguments = [ moduleName : project.getName()]
                }
            }
        }

        sourceSets {
            main {
                if (isModule.toBoolean()) {
                    manifest.srcFile 'src/main/module/AndroidManifest.xml'
                } else {
                    manifest.srcFile 'src/main/AndroidManifest.xml'
                    //集成开发模式下排除debug文件夹中的所有Java文件
                    java {
                        exclude 'debug/**'
                    }
                }
            }
        }

        buildTypes {
            release {
                minifyEnabled false
                proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
            }
        }

        lintOptions {
            abortOnError false
        }
    }

    dependencies {
        implementation fileTree(dir: 'libs', include: ['*.jar'])
        testImplementation 'junit:junit:4.12'
        //阿里的页面跳转路由翻译器
        annotationProcessor deps.arouter_compiler
        //引用公共库
        implementation 'com.icarbonx.demo:common:1.0.3'
    }

    apply from:"https://raw.githubusercontent.com/Shuaigoal/gradle/configs/upload-source-aar.gradle"
    //字符编码不统一，会造成出错
    tasks.withType(Javadoc) {
        options.encoding = "UTF-8"
    }
