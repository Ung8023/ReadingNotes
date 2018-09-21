# 使用添加`gradle`文件管理依赖库版本
```java
apply from: "xxxxx.gradle"
```

## 以前版本管理写法(`project-level`)

```java
buildscript {
    ext.kotlin_version = '1.1.51'
    repositories {
        google()
        jcenter()
    }
    dependencies {
        classpath 'com.android.tools.build:gradle:3.0.0'
        classpath "org.jetbrains.kotlin:kotlin-gradle-plugin:$kotlin_version"

        // NOTE: Do not place your application dependencies here; they belong
        // in the individual module build.gradle files
    }
}

allprojects {
    repositories {
        google()
        jcenter()
    }
}

task clean(type: Delete) {
    delete rootProject.buildDir
}

ext {
	......
	//定义xxxxx版本
}

```

## 将版本管理独立出来(在`project-level`添加`dependencies.gradle`)

```java
ext {

    //Android
    androidBuildToolsVersion = "26.0.2"
    androidMinSdkVersion = 21
    androidTargetSdkVersion = 26
    androidCompileSdkVersion = 26

    //Android Libraries
    supportVersion = "26.1.0"
    constraintLayoutVersion = "1.1.0"
    junitVersion = "4.12"
    testRunnerVersion = "1.0.1"
    testEspressoCoreVersion = "3.0.1"
    testMockitoVersion = "+"

    //3rd Libraries
    avLoadingIndicatorViewVersion = "2.1.3"
    retrofitVersion = '2.4.0'


    app = [
            supportV7                    : "com.android.support:appcompat-v7:${supportVersion}",
            constraintLayout             : "com.android.support.constraint:constraint-layout:${constraintLayoutVersion}",
            avLoadingIndicatorView       : "com.wang.avi:library:${avLoadingIndicatorViewVersion}",
            junit                        : "junit:junit:${junitVersion}",
            testRunner                   : "com.android.support.test:runner:${testRunnerVersion}",
            testEspressoCore             : "com.android.support.test.espresso:espresso-core:${testEspressoCoreVersion}",
            testMockito                  : "org.mockito:mockito-core:${testMockitoVersion}",

            retrofit                     : "com.squareup.retrofit2:retrofit:${retrofitVersion}",
            retrofitGsonConverter        : "com.squareup.retrofit2:converter-gson:${retrofitVersion}",
            retrofitScalarsConverter     : "com.squareup.retrofit2:converter-scalars:${retrofitVersion}",
    ]

}

```

## 让原来的项目级的`build.gradle`引用`dependencies.gradle`
```java
// Top-level build file where you can add configuration options common to all sub-projects/modules.
apply from: "dependence.gradle"

buildscript {
    ......
}

allprojects {
    repositories {
        google()
        jcenter()
    }
}

task clean(type: Delete) {
    delete rootProject.buildDir
}

```

## 修改`module-level`的`build.gradle`

```java
apply plugin: 'xxxxxx'

android {
    ......
}

dependencies {
	//在需要使用库的地方，定义一个名称，方便引用
    def androidBase = rootProject.ext.app

    implementation fileTree(dir: 'libs', include: ['*.jar'])

    testImplementation androidBase.junit
    androidTestImplementation androidBase.testRunner
    androidTestImplementation androidBase.testEspressoCore

    implementation androidBase.supportV7
    implementation androidBase.constraintLayout
}

```

