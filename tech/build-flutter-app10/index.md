# 利用Flutter做成应用10：打包应用


### 生成秘钥文件

要想把app发布到Play store，需要给app一个数字签名。

在windows，\[project]/android/app下执行下面的代码：
```
keytool -genkey -v -keystore .\Beauty.jks -storetype JKS -keyalg RSA -keysize 2048 -validity 10000 -alias Beauty
```
输入密码等信息，即可生成秘钥文件Beauty.jks，需要保持这个文件的私有性。
该命令会有一个Warning，建议迁移到PKCS12格式。

执行下面的代码，将Beauty.jks转换为PKCS12格式，原文件会备份成old格式。
```
keytool -importkeystore -srckeystore .\Beauty.jks -destkeystore .\Beauty.jks -deststoretype pkcs12
```

使用下面的命令可以查看秘钥文件的相关信息。
```
keytool -list -v -keystore .\Beauty.jks
```


### 在app中配置秘钥文件
在\[project]/android/创建文件key.properties，它包含了密钥文件相关的定义：

```java
storePassword=******
keyPassword=******
keyAlias=Beauty
storeFile=./Beauty.jks
```
该文件也需要保持私有性。 


修改\[project]/android/app/build.gradle文件，以通过gradle配置你的上传密钥。
```javascript
def keystoreProperties = new Properties()
def keystorePropertiesFile = rootProject.file('key.properties')
if (keystorePropertiesFile.exists()) {
    keystoreProperties.load(new FileInputStream(keystorePropertiesFile))
}

android {

    signingConfigs {
        release {
            keyAlias keystoreProperties['keyAlias']
            keyPassword keystoreProperties['keyPassword']
            storeFile keystoreProperties['storeFile'] ? file(keystoreProperties['storeFile']) : null
            storePassword keystoreProperties['storePassword']
        }
    }

    buildTypes {
        release {
            //signingConfig signingConfigs.debug
            signingConfig signingConfigs.release
        }
    }
}
```
之后编译发布版本就会自动签名了。  


### 构建发布版本

Android Studio中，使用Build->Flutter->Build App Bundle即可。
发布到Play Store时，推荐使用App bundle格式。

如需要更新版本，修改pubspec.yaml中的version值，再重新构建即可。



---

> Author:   
> URL: http://example.org/tech/build-flutter-app10/  

