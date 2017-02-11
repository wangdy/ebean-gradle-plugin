# Credit

Credit goes to khomich for his work https://github.com/khomich/gradle-ebean-enhancer - this plugin is based off that
(with updated enhancement, kapt support etc).

# ebean-gradle-plugin
Plugin that performs Enhancement (entity, transactional, query bean) and can generate query beans from entity beans written in Kotlin via kapt.

- Add `ebean-gradle-plugin` to buildscript/dependencies/classpath
- Add `apply plugin: 'ebean'`
- Add `generated/source/kapt/main` to sourceSets
- Add kapt generateStubs = true
- Add ebean plugin configuration

## Status

Currently using this with entity beans written in Kotlin (rather than Java).  I need to test the Java use.
There is also an issue with the Ebean IDEA plugin due to the unexpected directory structure (where ebean-typequery.mf goes).

## Example build.gradle

```groovy
group 'org.example'
version '1.0-SNAPSHOT'

buildscript {
    ext.kotlin_version = '1.0.6'
    ext.ebean_version = "10.1.5"
    ext.postgresql_driver_version = "9.4.1207.jre7"

    repositories {
        mavenCentral()
    }
    dependencies {
        classpath "org.jetbrains.kotlin:kotlin-gradle-plugin:$kotlin_version"
        classpath "com.github.wangdy:ebean-gradle-plugin:10.1.1"
    }
}

apply plugin: 'kotlin'
apply plugin: 'ebean'

repositories {
    mavenLocal()
    mavenCentral()
}

sourceSets {
    main.java.srcDirs += [file("$buildDir/generated/source/kapt/main")]
}

dependencies {

    compile "org.postgresql:postgresql:$postgresql_driver_version"
    compile "io.ebean:ebean:$ebean_version"
    compile "io.ebean:ebean-querybean:10.1.1"

    compile "org.jetbrains.kotlin:kotlin-stdlib:$kotlin_version"
}

kapt {
    generateStubs = true
}

ebean {
    debugLevel = 0 //1 - 9
    packages = ['org.example']
}

```
