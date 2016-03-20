##UINavigationController导航控制器初始化，导航控制器栈的push和pop跳转理解

（1）导航控制器初始化的时候一般都有一个根视图控制器，导航控制器相当于一个栈，里面装的是视图控制器，最先进去的在最下面，最后进去的在最上面。在最上面的那个视图控制器的视图就是这个导航控制器对外展示的界面，也就是用户看到的界面。 
（2）我们需要把导航控制器加载到APP中，需要把这个导航控制器设置为window的根视图控制器（都是控制器类，可以赋值），这样就相当于加载到了window里。 （3）我们要在栈中新增或者删除一个视图控制器，就需要得到导航控制器，一般在栈中得所有视图控制器都有一个self.navigationController，意思是我的导航控制器，也就是这个视图控制器所在的导航控制器，这样就拿到了导航控制器。 （4）栈中新增视图控制器用pushViewController，其实就是push进去一个，这样对于用户而言就是打开一个新界面了。 （5）栈中删除一个视图控制器用popViewControllerAnimated，当然这个pop只能pop最上面的那个，对于用户而言相当于从当前视图回到上一级视图。 （6）其实这个push和pop对于用户而言都是打开和跳转页面的一个操作。而pop由更多地操作方法，如一下子pop掉只剩下一个根视图控制器，那么就相当于从好几层直接回到最原始的主页面。也可以指定pop几个，以跳转到指定的页面。 （7）最重要的应该就是这个push和pop方法，而pop有很多种，这个理解后就不难记忆。

------------------------------------------------------------------------

（a）AppDelegate.m中，增加下面代码：
[objc] view plain copy 在CODE上查看代码片派生到我的代码片

    #import "AppDelegate.h"  
    //因为要实例化一个对象，需要这个类，所以导入  
    #import "ViewController.h"  
      
    @interface AppDelegate ()  
      
    @end  
      
    @implementation AppDelegate  
      
    - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {  
        //创建一个视图控制器，以届时作为导航控制器的根视图控制器  
        ViewController *root1=[[ViewController alloc]init];  
        //初始化导航控制器的时候把上面创建的root1初始化给它  
        UINavigationController *nav1=[[UINavigationController alloc]initWithRootViewController:root1];  
        //最后，我们把window的根视图控制器设为导航控制器，这样导航控制器就能够显示在屏幕上  
        self.window.rootViewController=nav1;  
        // Override point for customization after application launch.  
        return YES;  
    }  
      
    @end  

（b）在ViewController.m增加以下代码：

    [objc] view plain copy 在CODE上查看代码片派生到我的代码片
    #import "ViewController.h"  
    //因为需要实例化一个对象，所以导入它  
    #import "SecondViewController.h"  
      
    @interface ViewController ()  
      
    @end  
      
    @implementation ViewController  
      
    - (void)viewDidLoad {  
        //创建一个按钮，点击后进入子视图控制器，相当于进入子页面  
        UIButton *btn1=[UIButton buttonWithType:UIButtonTypeRoundedRect];  
        btn1.frame=CGRectMake(38, 100, 300, 30);  
        [btn1 setTitle:@"jump to secondviewcontroller" forState:UIControlStateNormal];  
        btn1.backgroundColor=[UIColor whiteColor];  
        self.view.backgroundColor=[UIColor redColor];  
        [btn1 addTarget:self action:@selector(jumpTo) forControlEvents:UIControlEventTouchUpInside];  
        [self.view addSubview:btn1];  
        [super viewDidLoad];  
        // Do any additional setup after loading the view, typically from a nib.  
    }  
      
    -(void)jumpTo{  
        //这里面核心的有两个,所谓跳转，其实就是往导航控制器栈中PUSH或者POP一个视图控制器，这样在最上面的视图控制器就变了，这样视图也跟着变了，因为只显示在栈顶得那个视图控制器的视图  
        //所以(1)控制所谓的跳转，其实是导航控制器在控制，在里面的元素都可以通过navigationController属性获取到它们所在的导航控制器  
        //所以(2)获取到导航控制器之后，使用Push的那个方法，往栈里面放一个视图控制器senCon1，这个新放入的在栈顶，就显示它的视图，所以用户改变页面跳转了  
        SecondViewController *senCon1=[[SecondViewController alloc]init];  
        [self.navigationController pushViewController:senCon1 animated:YES];  
    }  
      
    @end  

（c）在SecondViewController.m中增加以下代码：
 在CODE上查看代码片派生到我的代码片

    #import "SecondViewController.h"  
      
    @interface SecondViewController ()  
      
    @end  
    
    @implementation SecondViewController  
      
    - (void)viewDidLoad {  
        UILabel *label1=[[UILabel alloc]init];  
        label1.frame=CGRectMake(38, 80, 300, 30);  
        label1.backgroundColor=[UIColor whiteColor];  
        label1.text=@"This is secondviewcontroller";  
        [self.view addSubview:label1];  
          
        UIButton *btn2=[UIButton buttonWithType:UIButtonTypeRoundedRect];  
        btn2.frame=CGRectMake(38, 120, 300, 30);  
        [btn2 setTitle:@"backTo" forState:UIControlStateNormal];  
        btn2.backgroundColor=[UIColor orangeColor];  
        [self.view addSubview:btn2];  
        [btn2 addTarget:self action:@selector(backTo) forControlEvents:UIControlEventTouchUpInside];  
        [super viewDidLoad];  
        // Do any additional setup after loading the view.  
    }  
    //可以手动设置pop出栈，相当于删除这个页面，跳转到其他页面  
    //popViewControllerAnimated就是弹出，因为弹出只能弹出最上面的栈顶的那个，所以可以不用指定参数  
    //popToRootViewControllerAnimated-就是直接跳转到根视图控制图，如果只有两层，那么和popViewControllerAnimated并无区别，如果有很多层，那么其实就是相当于不仅把自己pop出去，还把所有除了根视图控制图之外的所有视图控制器都pop出去了，所以就相当于跳转到根视图控制器了  
    //popToViewController-就是跳转到指定的视图控制器xxx，这个xxx一定要在这个栈里面，即一定是在我们当前这个视图控制器的下面的，所以跳转也就是把自己和在xxx上面的所有视图控制器都pop出去，然后相当于直接跳转到xxx  
    //此处重点是这个xxx怎么获取，按照一般理解是用xxx再初始化一个视图控制器对象yyy，然后把这个对象yyy作为popToViewController参数  
    //但事实是，yyy是新初始化的，不在栈中，当然和在栈中的xxx初始化的那个对象也不是同一个对象，所以会报错（因为在栈中找不到啊）  
    //所以，self.navigationController.viewControllers出场，viewControllers是个数组，储存的时导航控制器栈中所有的视图控制器，最先push进去的时0，以此类推，最上面的肯定是数组的最后一个  
    //所以，那个xxx之前初始化的对象，可以用[self.navigationController.viewControllers objectAtIndex:0]表示，此处0就是根视图控制器  
    //所以，只要拿到navigationController，貌似能做很多事情  
    -(void)backTo{  
        [self.navigationController popToViewController:[self.navigationController.viewControllers objectAtIndex:0] animated:YES];  
    }  
      
    @end  



