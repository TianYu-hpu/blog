## Activity声明周期
　　一说到Activity生命周期，大家首先想到的是Android官方文档上的这个图片，相信很多人都已经知道这个方法的执行顺序，这里就不在赘述了，
![Activity 生命周期](http://7xp6n9.com1.z0.glb.clouddn.com/PY6X_K7_VNCHK%24TM~2CMEV.png)
　　除此之外，Activity还有其它方法，如onContentChanged,onPostCreate,onPostResume，onConfigurationChanged, onSaveInstanceState, onRestoreInstanceState,我们可以重写这几个方法，做一个demo来验证Activity的生命周期的方法。Activity启动运行并结束，Activity生命周期方法执行顺序是这样的：

	onCreate --> onContentChanged --> onStart --> onPostCreate --> onResume --> onPostResume --> onPause --> onStop --> onDestroy

　　onContentChanged是Activity中的一个回调方法，当Activity的布局改动时，级setContentView()或者addContentView()方法执行完毕时会调用该方法，例如Activity中各种findViewById()方法都可以防盗该方法中。
　　onPostCreate方法是指onCreate方法彻底执行完毕的回调，onPostResume类似，这两个方法官方说法是一般不会重写，现在找到的做法就是只有在使用ActionBarDrawerToggle的时候在OnPostCreate需要在屏幕旋转时候同步下状态，Google官方提供的一些实例就是如下做法：

	@Override
	protected void onPostCreate(Bundle savedInstanceState) {
		super.onPostCreate(savedInstanceState);
		// Sync the toggle state after onRestoreInstanceState has occurred.
		mDrawerToggle.syncState();
	}

　　onPause、onStop两者的区别:onPause是在整个窗口被半遮盖或者半透明的时候会执行，而onStop则是在整个窗口被完全遮盖才会触发，触发onStop方法之前必定会先触发onPause方法。
　　onCreate、onStart方法，onCreate方法会在第一次创建的时候执行，紧接着便会执行onStart方法，之后页面被完全遮挡会执行onStop方法，再返回的时候一般会执行onRestart --> onStart方法，但是如果这时候App内存不够需要更多内存的时候，App便会杀死该进程，结束掉该Activity，所以这时候再返回便会重新执行oncreate --> onStart --> onResume方法。
　　按下Home键的时候执行的过程  

	onPause  -->  onSaveInstanceState --> onStop
　　再次打开  

	onRestart --> onStart -->  onResume
　　屏幕旋转  
	* 如果不做任何配置，旋转屏幕，则Activity会被销毁并重新创建，之后便会执行如下方法：    

	onPause –> onSaveInstanceState –> onStop –> onDestroy –> onCreate –> onStart –> onRestoreInstanceState –> onResume
	* 在AndroidManifest配置文件里声明android:configChanges属性
默认屏幕旋转会重新创建，当然可以通过在配置文件里加上如下代码：  

	android:configChanges="keyboardHidden|orientation|screenSize"（sdk>13时需加上screenSize）
　　这个时候旋转屏幕不会再销毁Activity，这时候再旋转屏幕可以看到只会执行onConfigurationChanged方法，有什么在屏幕旋转的逻辑可以重写这个方法:

	public void onConfigurationChanged(Configuration newConfig) {
		if(newConfig.orientation == ActivityInfo.SCREEN_ORIENTATION_LANDSCAPE{
			//TODO:
		}
		super.onConfigurationChanged(newConfig);
	}
　　ActivityA打开ActivityB，这时候ActivityA生命周期的方法是这样的:onPause --> onSaveInstanceState --> onStop,这个时候在ActivityB按返回键，ActivityA会有以下几种情况:
	* 正常情况下会执行: onRestart --> onStart --> onResume
	* 当系统由于要回收内存而把Activity销毁时，Activity在 onPause或者onStop状态下都有可能遇到由于突发事件系统需要回收内存，之后的onDestroy方法便不会执行，这时候会执行onCreate --> onStart --> onRestoreInstanceState --> onResult    
#### onSaveInstanceState
　　onSaveInstanceState字面理解就是保存实例的状态，当某个Activity变得“容易”被系统销毁是，该Activity的onSaveInstanceState就会被执行，除非该Activity是被用户主动销毁的。  
　　何为"容易"呢，眼下之意就是该Activity还没有被销毁，而仅仅是一种可能性，这种可能性有这么几种情况:  
	1. 当用户按下HOME键时，这是显而易见的，系统不知道年下Home后要运行多少其它的程序，自然也不知道ActivityA是否会被销毁，故系统会调用onSaveInstanceState，让用户有机会保存某些非永久性的数据，一下几种情况的分析都遵循该原则    
	2.  长按HOME键，选择运行其它程序时
	3.  按下电源按键(关闭屏幕显示)时
	4.  从Activity A中启动一个新Activity时
	5.  屏幕方向切换时，例如从竖屏切换到横屏时
　　在屏幕切换之前，系统会销毁Activity A，在屏幕切换之后系统优惠自动创建ActivityA，所以onSaveInstanceState一定会执行
　　总而言之，onSaveInstanceState的调用遵循一个重要原则，即当系统"未经你许可"时销毁了你的Activity，则onSaveInstanceState会被系统调用，这是系统的责任，因为他必须要提供一个机会让你保存你的数据
#### onRestoreInstanceState
　　onRestoreInstanceState字面理解就是恢复实例的状态，需要注意的是，onSaveInstanceState和onRestoreInstanceState方法不一定是成对儿被调用的，onRestoreInstanceState被调用的前提是，ActivityA确实是被系统销毁了，而如果仅仅是停留在有这种可能性的前提下，则该方法不会被调用，例如，当显示ActivityA的时候，用户按下HOME键返回到主界面，然后用户紧接着又返回到ActivityA，这种情况下ActivityA一般不会因为内存的原因被系统销毁，故ActivityA的onRestoreInstanceState方法不会执行。
　　不过大多数情况下很少使用onRestoreInstanceState方法，经常我们还是在onCreate方法里直接恢复装填的，onCreate方法里本身有一个Bundle参数，很多时候我们是这样使用的.(onCreate在onStart之前调用，而onRestoreInstanceState是在onStart之后调用)

	protected void onSaveInstance(Bundle saveInstanceState) {
		super.onSaveInstanceState(savedInstanceState);
		savedInstanceState.putLong("param", value);
	}

	public void onCreate(Bundle savedInstanceState) {
		if(savedInstanceState != null) {
			value = savedInstanceState.getLong("param");
		}
	}