# Gradle Project Example

## PreRequirements

* First of ALL, save `init.gradle` to you ~/.gradle/init.gradle

* It will use your `.m2/settings.xml` to identify the deployer's credentials.

## Features

* `gradle publish`

Automatically publish your artifact's to nexus repository, according to the **SUFFIX** of the artifact.

* `gradle cV` or `gradle currentVersion`

Powered by [Axion Release](https://github.com/allegro/axion-release-plugin)

## Usage

In your project's **root** `build.gradle`:


    buildscript {
        dependencies {
            classpath 'org.hibernate.build.gradle:gradle-maven-publish-auth:2.0.1'
        }
    }
    
    plugins {
        id 'pl.allegro.tech.build.axion-release' version '1.5.0'
        // ... any other plugins
    }
    
    // your group same as maven's group
    group = '...'
    
    // your version definitions
    ext {
        //...  
    }
    
    // for Axion Release plugin use
    scmVersion {
        tag {
            prefix = 'your tag prefix'
        }
    }
    
    // all project's configuration
    allprojects {
        project.version = scmVersion.version
        apply plugin: 'maven-publish-auth'
        
        // ...
    }
    
    subprojects {
    
         // for gradle-maven-publish-auth plugin use
         buildscript {
             dependencies {
                 classpath 'org.hibernate.build.gradle:gradle-maven-publish-auth:2.0.1'
             }
         }
         
         //...
    
         // for gradle task: gradle publish
         publishing {
            publications {
                mavenJava(MavenPublication) {
                    from components.java
                }
            }
         }
    }
    
 ## `~/.gradle/init.gradle`
 
     allprojects {
     
         apply plugin: 'maven-publish'
     
         buildscript {
             repositories {
                 mavenLocal()
                 maven {
                     url "http://nexus.example.com/repository/maven-public/"
                 }
             }
         }
     
         repositories {
             mavenLocal()
             maven {
                 url "http://nexus.example.com/repository/maven-public/"
             }
         }
     
         publishing {
             repositories{
                 maven {
                     if(project.version.endsWith('-SNAPSHOT')) {
                         name "nexus.example.com-snapshots"
                         url "http://nexus.example.com/repository/maven-snapshots/"
                     } else {
                         name "nexus.example.com-releases"
                         url "http://nexus.example.com/repository/maven-releases/"
                     }
                 }
             }
         }
         
     }

    
## `~/.m2/settings.xml`


    <settings>
        <servers>
            <server>
                <id>nexus.example.com-snapshots</id>
                <username>developer</username>
                <password>developer's password</password>
            </server>
            <server>
                <id>nexus.example.com-releases</id>
                <username>admin</username>
                <password>admin's passwprd</password>
            </server>
        </servers>
        
    </settings>