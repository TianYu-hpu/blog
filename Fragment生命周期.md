## Fragment生命周期
![](http://7xp6n9.com1.z0.glb.clouddn.com/QQ%E5%9B%BE%E7%89%8720160110144136.png)
1. 运行状态  
　　当Fragment是可见的，并且它所关联的Activity处于运行状态时，该Fragment也处于运行状态  
2. 暂停状态  
　　当一个Activity处于暂停状态(由于另一个未占满屏幕的Activity被添加到了栈顶)，与它相关联的可见的Fragment就会进入到暂停状态。  
3. 停止状态  
　　当一个Activity进入停止状态时，与它相关联的碎片也进入停滞状态。或者通过调用FragmentTransaction的removed()、replace() 方法将Fragment从Activity中移除，但有在事务之前调用addToBackStack()方法，这是Fragment也会进入到停止状态。总的来说，进入停止状态的碎片对用户来说是完全不可见的，有可能被系统回收。
4. 销毁状态  
　　Fragment总是依附于Activity而存在的，因此当Activity被销毁时，与它相关联的Fragment就会进入到销毁状态。或者通过调用FragmentTransaction的removed()/replace()方法将碎片从Activity中移除，但在事务提交之前并没有调用addToBackStack()方法，这时碎片也会进入到销毁状态。
　　Fragment提供了一些回调方法，以覆盖Fragment生命周期的各个环节
　　onAttach():当Activity与Fragment建立关联的时候调用。
　　onCreateView():为Fragment创建视图(加载布局)时调用
　　onActivityCreated():确保与Activity相关联的Activity已经创办完毕的时候调用。
　　onDestroyView():当与Fragment相关联的视图被移除的时候调用。
　　onDetach():当Fragment和Activity接触关联的时候调用。