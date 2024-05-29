# 修改内容

- 在运行测试时，发现功能缺失的现象，在查找资料后，解决此问题

  链接：[1](https://blog.csdn.net/m0_66137457/article/details/136762065)

  定位：app->res->values->styles
  在styles.xml中下滑到最后
  修改代码为：

  ```html
  <style name="NoteActionBarStyle" parent="@android:style/Widget.Holo.Light.ActionBar.Solid">
   <!--    <item name="android:displayOptions" />-->
       <item name="android:visibility">visible</item>
   </style>
  ```

  

- DataUtils.java

  - 可以防止在`cursor`已经是`null`时尝试关闭它而导致的`NullPointerException`
  - 减少冗余代码和嵌套层次可以降低出错的可能性，并使代码更易于维护
  - 减少了代码的嵌套，使逻辑更加清晰

  ```java
  if(cursor != null) {
      if(cursor.moveToFirst()) {
          try {
              count = cursor.getInt(0);
          } catch (IndexOutOfBoundsException e) {
              Log.e(TAG, "get folder count failed:" + e.toString());
          } finally {
              cursor.close();
          }
      }
  }
  ```

  改为：

  ```java
  if(cursor != null && cursor.moveToFirst()) {
      try {
          count = cursor.getInt(0);
      } catch (IndexOutOfBoundsException e) {
          Log.e(TAG, "get folder count failed:" + e.toString());
      } finally {
          cursor.close();
      }
  }
  ```

- DateTimePickerDialog.java

  - 重复使用了 `DateUtils.FORMAT_24HOUR`

  ```java
  int flag =
      DateUtils.FORMAT_SHOW_YEAR |
      DateUtils.FORMAT_SHOW_DATE |
      DateUtils.FORMAT_SHOW_TIME;
  flag |= mIs24HourView ? DateUtils.FORMAT_24HOUR : DateUtils.FORMAT_24HOUR;
  ```

  猜测打错了，改为：

  ```java
  int flag =
      DateUtils.FORMAT_SHOW_YEAR |
      DateUtils.FORMAT_SHOW_DATE |
      DateUtils.FORMAT_SHOW_TIME;
  flag |= mIs24HourView ? DateUtils.FORMAT_24HOUR : DateUtils.FORMAT_12HOUR;
  ```

  

# 运行

1. 直接使用Android studio打开即可
2. Android studio创建新项目，
   Package name设定为net.micode.notes
   Language选择Java
   Build configuration language个人选用的是Groovy DSL
   将代码文件中的notes下文件放入路径 `\app\src\main\java\net\micode\notes`
   res下文件放入路径`\app\src\main\res`
   app下build.gradle文件替换
   `\app\src\main`下AndroidManifest.xml文件替换即可

