# CitySelect
仿照美团地址选择器


### 依赖库

#### build.gradle 用于打包aar
```java
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
```
### 在项目中使用
#### 第一种方式：Activity

第一种方式不是很方便，应该是不是很灵活，用的是启动跳转到库里的activity：com.gavin.city.citylibrary.CityPickerActivity
这个方式，我们需要在AndroidMenifest.xml中对Activity进行注册
```java
  <activity android:name="com.gavin.city.citylibrary.CityPickerActivity"/>
  ```
  在代码中的点击事件中跳转
  ```java
  Intent intent = new Intent(MainActivity.this, CityPickerActivity.class);
                intent.putExtra("city","深圳市");
                startActivityForResult(intent,REQUEST_CODE_PICK_CITY);
                ```
 注释：因为本库相对于别的类似的库去掉了定位功能，减少包的大小，但前位置需要用户自己传入 intent.putExtra("city","深圳市");
 
 返回：选中所选的地方库里的操作city是返回的所选城市
 ```java
 Intent data = new Intent();
        data.putExtra(KEY_PICKED_CITY, city);
        setResult(RESULT_OK, data);
        finish();
        ```
### 方式2：fragment
相对于Activity，fragment是比较方便点，看个人喜好吧
