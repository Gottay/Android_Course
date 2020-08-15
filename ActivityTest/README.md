* app目录下的build.gradle文件
![](/Users/gotta/Desktop/app-build.gradle.png)
　　第一行可以看到引入了一个插件，一般有两个值可以选择：`com.android.application` 表示这是个应用程序模块；`com.android.library`表示这是一个库模块。两者最大的区别在于，一个是可以 **直接运行的** ，一个只能 **依附于其他应用程序而运行** 。<br>
　　第三行开始是一个android闭包，在这个闭包中我们可以配置项目构建的各种属性。`compileSdkVersion`用于指定项目编译版本；`buildToolsVersion`用于指定项目构建工具版本。<br>
　　在这个android闭包中可以看到又有一个defaultConfig闭包。`applicationId`用于指定项目的包名；`minSdkVersion`用于表示最低兼容的Android版本；`versionCode`用于指定项目的版本号，`versionName`用于指定项目的版本名，这两个属性在安装软件时非常重要。

*  配置主活动，因为程序启动时需要知道首先启动的哪个程序

```
<intent-filter>
	<action android:name="android.intent.action.MAIN"/>
	<category android:name="android.intent.category.LAUNCHER"/>
</intent-filter>
```

*	Toast是Android系统提供的一种非常好的提醒方式，可以将一些短小的消息通知给用户，过一段时间这些消息会自动消失，并且 **不会占用** 任何屏幕空间。

	这里我们实现点击button时弹出一个toast消息，所以在onCreate()方法中加入以下代码：
	
	```
	//首先找到布局文件中定义的button元素
	Button button1 = (Button) findViewById(R.id.button_1);  
	//为button元素设置一个监听器，当点击该button时就会调用其中的onClick( )方法
      button1.setOnClickListener(new View.OnClickListener(){ 
            @Override
            public void onClick(View v){
                Toast.makeText(FirstActivity.this, "You clicked Button 1",
                        Toast.LENGTH_SHORT).show();
            }
        });
	```
	在上面的代码中Toast函数通过 makeText( ) 静态方法创建一个对象，然后调用 show( ) 将Toast显示出来； makeText( ) 方法需要传入3个参数，第一个为 Context ，即 Toast 要求的上下文，由于 **活动本身就是一个Context** 所以直接传入FirstActivity.this即可；第二个参数是 Toast 消息显示的文本内容；第三个参数是Toast显示时长，有两个内置常量：`Toast.LENGTH_SHORT` 和 `Toast.LENGTH_LONG`
	
*	在活动中使用Menu
	
	在 `res` 文件下下新建 `menu` 文件夹，新建main.xml菜单源文件
	
	```
	<menu xmlns:android="http://schemas.android.com/apk/res/android">
    		<item
        		android:id="@+id/add_item"
        		android:title="Add"/>
		<item
        		android:id="@+id/remove_item"
       			android:title="Remove"/>
	</menu>
	```
	这里创建了两个菜单项，用 `id` 进行唯一标识， `title` 则为菜单栏显示的文字。
	
	接着在 FirstActivity 中重写 `onCreateOptionsMenu( )` 方法，这个方法负责生成menu，它是一个回调方法，当按下手机设备上的menu按键的时候，android系统才会生成一个包含两个子项的菜单。
	
	**<显示菜单>**<br>
	
	```
	@Override
    	public boolean onCreateOptionsMenu(Menu menu) {
      		getMenuInflater().inflate(R.menu.main, menu);
        	return true;
   	 }
	```
	其中 `getMenuInflater( )` 方法可以的得到一个 MenuInflater 对象，然后该对象调用 inflate( ) 方法就可以给当前活动创建菜单了。
	
	**<菜单响应事件>**<br>
	在 FirstActivity 中重写 `onOptionsItemSelected( )` 方法
	
	```
	@Override
    	public boolean onOptionsItemSelected(@NonNull MenuItem item) {
      		switch(item.getItemId()){
      			case R.id.add_item:
      				Toast.makeText(this, "You clicked add", Toast.LENGTH_SHORT).show();
      				break;
      			case R.id.remove_item:
      				Toast.makeText(this, "You clicked remove", Toast.LENGTH_SHORT).show();
      				break;
      			default:
      		}
        return true;
    }
	```