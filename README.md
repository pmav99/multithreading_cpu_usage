# python_multithreading_system_cpu_usage

This example shows that you can have high `system CPU usage` while running
multi-threaded code under Python 2.

1. Open a console and run `sudo apt install sysstat`
2. On the same console run `sar 1`. Let it running
2. On a new console console run `sar -w 1`. Let it running. This is showing you the
   number of context switches per second.
3. On a new console execute `python2 single_threaded.py`
4. Observer the results on `sar`. On my machine, which is a 4core/8threads machine, the
   User CPU usage is at 12.5% (i.e. 1/8). The system CPU usage was practically unchanged. The context switches the same.
5. Now execute `python2 multi_processing.py`
6. Observer the results. The User CPU usage should be ~ 25% (we run two processes)
   but system CPU usage should only increase by 1% or so. The context switches should be
   almost the same.
5. Now execute `python2 multi_threaded.py`
6. Observer the results. The User CPU usage should be at 12.5% (we only have one process
   running after all) but you should now have significant system CPU usage too (on my
   machine around 7-8%). The context switches should be 50k-70k! For the record, the
   system CPU usage is caused by the context swiches.
5. Finally execute `python3 multi_threaded.py`
6. Observer the results. The User CPU usage should be ~ 12.5% but the system
   CPU usage should remain around 1% while the context switches should also be normal.

If you want to know why this is happening, the short answer is that we create 2 threads which are contesting to run on the same process.
For a little bit more context why this is happening check the answers in this
[thread](https://stackoverflow.com/questions/5426533/python-threading-lock-slows-my-app-down-considerably/5441992#5441992).
And if you truly want more info, then check [this presentation](https://www.dabeaz.com/GIL/)
