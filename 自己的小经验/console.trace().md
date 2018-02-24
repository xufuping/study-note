## console.trace()

**console.trace()函数**用来打印函数调用的栈信息

    function doTask(){
        doSubTask(1000,10000);
    }
 
    function doSubTask(countX,countY){
        for(var i=0;i<countX;i++){
            for(var j=0;j<countY;j++){} 
        }
        console.trace();
    }
    doTask();
    
控制台就是这样的：

![image](http://files.jb51.net/file_images/article/201412/20141229102759864.png?2014112910288)