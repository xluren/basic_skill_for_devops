####CPU
======
	内核主要调度 CPU 对中断和进程（线程）进行处理，线程又分为内核线程和用户级线程。把它们按 CPU 处理的优先级由高到低进行排序：中断、内核线程、用户级线程。
	
	当线程用完自己的时间片或者需要等待 IO，则发生上下文切换，CPU 将调度其它线程运行。上下文切换是一个耗时的操作，如果CPU 忙于上下文切换，则可用于处理实际有意义的工作的时间自然就会变少。
	
	当线程处于可运行状态，但 CPU 被其它线程占用因而暂时无法被调度时，将被放进运行队列中等待。运行队列的长度通常直接反映了系统负载状态。uptime中的load在CPU中可以理解为CPU可以并行处理的任务数，那么就是"CPU个数 * 核数"，如果CPU Load = CPU个数 * 核数 那么就是说CPU正好满负载，再多一点，可能就要出问题了，有任务不能被及时分配处理器，那么保证性能的话，最好是小于CPU个数 * 核数 *0.7。可见，当 load 值与 CPU 数（核、超线程）相同时系统处于最佳 CPU 利率状态（如果多个CPU被均衡使用），所有可运行的线程都能被即时调度。
	
	CPU 的利用率通常分为以下几种：
		us (user time)，运行用户级线程的时间，通常在进行实际的业务处理。
		sy (system time)，运行内核线程的时间，上下文切换等。
		wa (wait io)，等待 IO 的时间。
		si (soft irq)，处理软中断时间。
		id (idle)，CPU 空闲。
		对于 CPU 利用率的正常状态有以下几点预期：
			1.每个 CPU 上的运行队列长度不应超过 3（最好尽可能不在运行队列中堆积线程，在 load 值达到 CPU 数之间就应进行扩容）。
			2.如果 CPU 已经被完全利用（100%），则应该呈现如下分布：65%~70%的 us，30%~35%的 sy 以及0%~5%的 id。
			3.如果 CPU 利用率满足上述分布，那么相对频繁的上下文切换是可以接受的。
		通常可以使用 vmstat 和 top 来查看 CPU 的使用情况，mpstat 可以看到每个 CPU 的使用情况（top 也可以）。
		进行 CPU 监控和问题排查时主要有以下几点：
		检查运行队列长度（load 值）。
		检查 CPU 利用率是否满足前面所述的分布。
		如果 CPU 在系统态(sy)花费了过多时间，通常说明系统过载了。
		内核调度器会“惩罚” CPU 密集型进程，“奖励” IO 密集型进程。
分析系统cpu问题过程：
	方法一:
		1.用top命令查看哪个进程占用CPU高
		2.用top -H -p pid命令查看进程内各个线程占用的CPU百分比
		3.使用gstack命令查看进程中各线程的函数调用栈
		4.使用gcore命令转存进程映像及内存上下文
		5.用strace命令查看系统调用和花费的时间 
			常用: strace -o output.txt -T -tt -e trace=all -p $pid
		6.用gdb调试core文件
	方法二：
		1、top命令：Linux命令。可以查看实时的CPU使用情况。也可以查看最近一段时间的CPU使用情况。
		2、PS命令：Linux命令。强大的进程状态监控命令。可以查看进程以及进程中线程的当前CPU使用情况。属于当前状态的采样数据。
		3、jstack：Java提供的命令。可以查看某个进程的当前线程栈运行情况。根据这个命令的输出可以定位某个进程的所有线程的当前运行状态、运行代码，以及是否死锁等等。
		4、pstack：Linux命令。可以查看某个进程的当前线程栈运行情况。