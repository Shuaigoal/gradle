# gradle
build gradle properties files templates

Modify the project build.gradle with common version control

At project to level build.gradle, modify 

// Top-level build file where you can add configuration options common to all sub-projects/modules.

buildscript {

    apply from: 'https://raw.githubusercontent.com/Shuaigoal/gradle/common/common_library.gradle'
    
    //apply from: 'common_library.gradle'
    
    addRepos(repositories)
    
    dependencies {
    
        /* classpath deps.android_gradle_plugin*/
        
        classpath deps.android_gradle_plugin
        
        classpath deps.kotlin.plugin
        
        // NOTE: Do not place your application dependencies here; they belong
        // in the individual module build.gradle files
    }
}
   
   
allprojects {

    addRepos(repositories)
    
    // Android dependency 'com.android.support:design' has different version for the compile (25.3.1) and runtime (25.4.0) classpath.
    // You should manually set the same version via DependencyResolution
    
    subprojects {
    
        project.configurations.all {
        
            resolutionStrategy.eachDependency { details ->
            
                if (details.requested.group == 'com.android.support'
                
                        && !details.requested.name.contains('multidex')) {
                        
                    details.useVersion  "27.0.2"
                    
                }
                
            }
            
        }
        
    }
}
