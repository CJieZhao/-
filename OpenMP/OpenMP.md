# OpenMP
---
## 常用函数说明
| 功能 | 函数声明 | 函数说明 |
|--|--|--|
| 设置线程数目 | void omp_set_num_threads(int num_threads) | 通过该函数来指定其后用于并行计算的线程数目，其中参数num_threads就是指定的线程数目 |
| 获取线程数目 | int omp_get_num_threads() | 通过该函数可以获取当前运行组中的线程数目，如果是在并行结构中使用该函数，其返回的就是现在并行计算中的所有的线程总数，如果是在串行中使用该函数，其返回值就为1 |
| 获取最多线程数目 | int omp_get_max_threads() | 该函数将返回最多可以用于并行计算的线程数目 |
| 返回线程ID | int omp_get_thread_num() | 通过该函数可以返回当前线程的ID，如果使用该函数时处于并行结构中，它返回的就是这个并行线程的ID，如果在串行中，就是返回主线程的ID |
| 获取程序可用的处理器数目 | int omp_get_num_procs() | 获取程序可用的处理器数目 |
| 获取时间 | double omp_get_wtime() | 该函数返回为时钟运行的时间，单位为s，如果现在时刻为11:40:30.8, 则该值为11\*3600+40\*60+30.8=42030.8 在程序运行开始和即将结束时使用调用这个函数可以用于计算程序运行的时间 |
| 是否处于并行中 | int omp_in_parallel() | 该函数返回值为0表示现在处于串行程序中，值为1表示现在处于并行程序中 |
| 开启嵌套并行 | void omp_set_nested(int) | 参数为1时,开启嵌套并行 |

## 主要关键字(测试环境VS2013)

### 并行运行
1. _OPENMP 条件编译, OPENMP 环境定义_OPENMP
	
	```
	#ifdef _OPENMP
	    printf("OPENMP\n");
	#endif
	```
1. \#pragma omp parallel 一般跟大括号使用, 大括号内为并行内容.
	
	``` 
	int x = 100;
   	#pragma omp parallel
  	{
        	if (omp_get_thread_num() == 3) {
	            x = 10;
        	} else {
	            printf("x = %d\n", x);
        	}
	}
	```
1. \#pragma omp parallel for 并行循环

	```
	#pragma omp parallel for
	for (int i = 0; i < 10; ++i) {
		printf("-");
	}
	```
1. \#pragma omp parallel for reduction(+:sum) 并行求和

	``` 
	#pragma omp parallel for reduction(+:sum)
	for (int i = 0; i < 100; ++i) {
		sum += i;
	}
	```
1. \#pragma omp parallel num_threads(N) 并行指定并行线程数

	```
	#pragma omp parallel num_threads(2)
	{
        	printf("%d : %d\n", __LINE__, omp_get_thread_num());
	}
	```
1. \#pragma omp parallel sections 并行并指定一个线程的工作内容<BR>[采用section定义的每段程序都将只执行一次，sections中的每段section将并行执行。一个程序中可以定义多个sections，每个sections中又可以定义多个section。同一个sections中section之间处于并行状态。sections与其他sections之间处于串行状态采用section定义的每段程序都将只执行一次，sections中的每段section将并行执行。一个程序中可以定义多个sections，每个sections中又可以定义多个section。同一个sections中section之间处于并行状态。sections与其他sections之间处于串行状态]
	
	``` 
    #pragma omp parallel sections
    {
		/// 下个 for 由一个线程执行
		#pragma omp section
		for (int i = 0; i < 5; ++i) {
			printf("+ : %d\n", omp_get_thread_num());
		}

		#pragma omp section
		for (int i = 0; i < 5; ++i) {
			printf("- : %d\n", omp_get_thread_num());
		}
	}
	```

1. \#pragma omp single 只有一个线程执行
	
	```
	#pragma omp single
    {
		printf("Single\n");
	}
	```

1. \#pragma omp parallel if () 根据条件判断是否采取并行, true为并行, false为串行
	```
	bool isTrue = !false;
    #pragma omp parallel if (isTrue)
    {
        printf("\n");
    }
	```

### 并行同步

1. \#pragma omp master 指定主线程工作内容
1. \#pragma omp critical 并行中线程临界区块
1. \#pragma omp barrier 强制等待线程组中全部线程运行到此处
1. \#pragma omp for nowait 去掉循环结束的强制同步

## 使用心得

1. 嵌套并行情况(_也包括并行调用函数, 函数内部有并行情况_), 默认情况下内层并行会按照串行执行(_测试发现由0线程执行). 可以使用 omp_set_nested(1) 函数开启嵌套并行
	
    ```
    #pragma omp parallel for
    for (int i = 0; i < 10; ++i) {
        #pragma omp parallel for
        for (int j = 0; j < 10; ++j) {
            printf("%d ", omp_get_thread_num());
        }
        printf("\n");
    }
	```

2. 最适用的环境, 每次循环不依赖之前循环结果. 相当于每次循环都是相对独立的情况.

	```
	#pragma omp parallel for
	for (int i = 0; i < 10; ++i) {
		printf("id = %d\n", omp_get_thread_num());
	}
	```

3. 线程开启后未找到关闭的方法, 启动后线程数一直不减少.





















