## Activity启动模式
　　Activity的启动模式对我们来说是个全新的概念，在实际项目中我们应该根据特定的需求为每个Activity指定恰当的启动模式。启动模式一共有四种，分别是standard、singleTop、singleTask和singleInstance,可以在AndroidManifest.xml文件中通过<activity>标签指定android:launchMode属性来选择启动模式。
　　1. standard  
　　　　standard是Activity默认的启动模式，在不进行显示的指定的情况下，所有Activity都会自动使用这种启动模式。因此到目前为止我们遇到的所有Activity使用的都是standard模式。在standard模式下，每当启动一个新的Activity，它就会在返回栈中入栈，并处于栈顶的位置，对于使用standard模式的Activity，系统不会在乎这个Activity是否已经在返回栈中存在，每次启动都会创建该Activity的一个新的实例。我们通过代码来体会一下standard模式。  

	@Override
	protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		Log.d("FirstActivity", this.toString());
		requestWindowFeature(Window.FEATURE_NO_TITLE);
		setContentView(R.layout.first_layout);
		Button button1 = (Button) findViewById(R.id.button_1);
		button1.setOnClickListener(new OnClickListener() {
			@Override
			public void onClick(View v) {
				Intent intent = new Intent(FirstActivity.this,FirstActivity.class);
				startActivity(intent);
			}
		});
	}
　　　　代码看起来有些奇怪，在FirstActivity的基础上启动FirstActivity。从逻辑上来讲确实没什么意义，不过重点在于研究standard模式，另外在onCreate方法中添加了一行打印信息，用于打印当前Activity的实例。运行程序，在FirstActivity界面连续点击两次按钮，日志信息如图所示:

　　　　从日志信息中我们可以看出，每点击一次按钮就会创建出一个新的Activity实例，此时返回栈中存在三个FirstActivity的实例，因此你需要连按三次返回键才能推出程序。standard模式原理示意图如下:

　　2. singleTop  
　　　　在有些情况下，使用standard模式不太合理，Activity明明已经在栈顶了，为什么再次启动Activity的时候还要创建一个新的Activity呢？这是系统模式的一种启动模式而已，完全可以根据自己的需要进行修改。当启动模式指定为singleTop，在启动Activity时如果发现返回栈的栈顶已经是该Activity了，则认为可以直接使用它，不会再创建新的Activity实例。而是会调用其onNewIntent方法。不过当Activity未处于栈顶时，再启动Activity还是会创建新的Activity，修改FirstActivity中的onCreate方法来验证一下。  
		
	@Override
	protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		Log.d("FirstActivity", this.toString());
		requestWindowFeature(Window.FEATURE_NO_TITLE);
		setContentView(R.layout.first_layout);
		Button button1 = (Button) findViewById(R.id.button_1);
		button1.setOnClickListener(new OnClickListener() {
			@Override
			public void onClick(View v) {
				Intent intent = new Intent(FirstActivity.this,
				SecondActivity.class);
				startActivity(intent);
			}
		});
	}
　　　　这次点击按钮启动SecondActivity，然后在SecondActivity中启动FirstActivity  

	protected void onCreate(Bundle savedInstanceState) {
	super.onCreate(savedInstanceState);
		Log.d("SecondActivity", this.toString());
		requestWindowFeature(Window.FEATURE_NO_TITLE);
		setContentView(R.layout.second_layout);
		Button button2 = (Button) findViewById(R.id.button_2);
		button2.setOnClickListener(new OnClickListener() {
			@Override
			public void onClick(View v) {
				Intent intent = new Intent(SecondActivity.this,
				FirstActivity.class);
				startActivity(intent);
			}
		});
	}
　　　　运行程序查看日志信息：

　　　　可以看到系统创建了两个不同的FirstActivity实例，这是由于SecondActivity中再次启动FirstActivity时，栈顶Activity已经变成了SecondActivity，因此会创建一个新的FirstActivity实例。现在按下返回键会回到SecondActivity，再次按下返回键又会回到FirstActivity，再按一次返回键退出程序，singleTop模式的原理示意图：　　

　　3. singleTask  
　　　　使用singleTop模式可以很好的解决重复创建栈顶Activity的问题，但是如果该Activity没有处于栈顶的位置，还是可能会创建多个Activity实例，那么有没有什么办法可以让某个Activity在整个应用程序中的上下文中只存在一个实例呢？这要借助singleTask模式实现，当Activity的启动模式设置为singleTask，每次启动该Activity时首先会在返回栈中检查是否存在该Activity的实例，如果发现已经存在则直接使用该实例，并把在这个Activity之上的所有Activity统统出栈，如果没有发现就创建一个新的Activity实例。  
　　　　在FirstActivity中添加onRestart方法，打印日志:

	@Override
	protected void onRestart() {
		super.onRestart();
		Log.d("FirstActivity", "onRestart");
	}
　　　　最后在SecondActivity中添加onDestroy方法，并打印日志:

	@Override
	protected void onDestroy() {
		super.onDestroy();
		Log.d("SecondActivity", "onDestroy");
	}
　　　　运行程序，在FirstActivity界面点击按钮进入到SecondActivity，然后在SecondActivity界面点击按钮，又会重新进入到FirstActivity。查看打印信息:

　　　　从打印信息中可以看出，在SecondActivity中启动FirstActivity时，会发现返回栈中已经存在一个FirstActivity的实例，并且是在SecondActivity的下面，于是SecondActivity会从返回栈中出栈，而FirstActivity重新成为了栈顶活动，因此FirstActivity的onRestart方法和SecondActivity的onDestory方法就会得到执行。现在反悔栈中只剩下一个实例了，按一下返回键就可以退出程序了。singleTask原理图:
  
　　4. singleInstance  
　　　　singleInstance模式是四中启动模式中最特殊最复杂的一个，指定为singleInstance模式的Activity会启动一个新的返回栈来管理活动，(其实如果singleTask模式制订了不同的task Affinity,也会启动一个新的返回栈)这样做有什么意思呢，想想一下场景，假设程序中有一个活动是允许其他程序调用的，如果我们想实现其他程序调用我们的程序可以共享这Activity的实例，该如何实现呢？前面三种启动模式肯定是做不到的，因为每个应用都会有自己的返回栈，同一个Activity在不通的返回栈中入栈时必然是创建了新的实例，使用singleInstance模式就可以解决这个问题，这种模式会有一个单独的返回栈来管理这个Activity，不管是哪个应用程序来访问这个Activity，都公用的是同一个返回栈，也就解决了共享Activity实例的问题。  
　　　　我们将SecondActivity的启动模式改为singleInstance，然后我们修改FirstActivity中的onCreate方法。

	@Override
	protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		Log.d("FirstActivity", "Task id is " + getTaskId());
		requestWindowFeature(Window.FEATURE_NO_TITLE);
		setContentView(R.layout.first_layout);
		Button button1 = (Button) findViewById(R.id.button_1);
		button1.setOnClickListener(new OnClickListener() {
			@Override
			public void onClick(View v) {
				Intent intent = new Intent(FirstActivity.this,
				SecondActivity.class);
				startActivity(intent);
			}
		});
	}
　　　　在onCreate方法中打印了当前返回栈的id,然后修改SecondActivity中的onCreate方法:

	@Override
	protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
			Log.d("SecondActivity", "Task id is " + getTaskId());
			requestWindowFeature(Window.FEATURE_NO_TITLE);
			setContentView(R.layout.second_layout);
			Button button2 = (Button) findViewById(R.id.button_2);
			button2.setOnClickListener(new OnClickListener() {
			@Override
			public void onClick(View v) {
				Intent intent = new Intent(SecondActivity.this,
				ThirdActivity.class);
				startActivity(intent);
			}
		});
	}
　　　　同样在onCreate方法中返回了当前的返回栈id，然后又修改了按钮点击事件代码，用于启动ThirdActivity,最后修改ThirdActivity中的onCreate方法。

	@Override
	protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		Log.d("ThirdActivity", "Task id is " + getTaskId());
		requestWindowFeature(Window.FEATURE_NO_TITLE);
		setContentView(R.layout.third_layout);
	}
　　　　依然在onCreate方法中打印了当前返回栈的id,重新运行程序，在SecondActivity界面点击按钮进入到SecondActivity，在SecondActivity界面点击按钮进入到ThirdActivity，打印日志信息:

　　　　可以看到，SecondActivity的Task id不同于FirstActivity和ThirdActivity，这说明SecondActivity确实是存放在一个单独的返回栈中，而且这个栈中只有一个SecondActivity。
　　　　我们按下返回键会发现ThirdActivity竟然直接返回到了FirstActivity，在按下返回键又返回到了SecondActivity，再按下返回键退出程序。这是为什么呢？原理很简单，由于FirstActivity和ThirdActivity是存放在同一个返回栈里的，当在ThirdActivity的界面按下Back键，ThirdActivity会从返回栈中出栈，那么FirstActivity就成为了栈顶Activity显示在界面上，因此也就出现了从ThirdActivity直接返回到FirstActivity的情况。然后在FirstActivity界面再次按下返回键，这时当前的返回栈已经空了，于是就显示了另一个返回栈的栈顶Activity，即SecondActivity。最后再次按下返回键，这时所有的返回栈都已经空了。也就自然退出了程序。singleInstance原理示意图:
