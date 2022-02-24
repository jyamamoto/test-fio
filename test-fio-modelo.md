# Test modelo etcd 
~~~
fio --rw=write --ioengine=sync --fdatasync=1 --directory=test-data --size=22m --bs=2300 --name=mytest
~~~

* ioengine=sync - Basic read(2) or write(2) I/O. lseek(2) is used to position the I/O location. See fsync and fdatasync for syncing write I/Os.
* fdatasync=int - Like fsync but uses fdatasync(2) to only sync data and not metadata blocks. In Windows, DragonFlyBSD or OSX there is no fdatasync(2) so this falls back to using fsync(2). Defaults to 0, which means fio does not periodically issue and wait for a data-only sync to complete.

# JC test style - Random Write 
~~~
fio --ioengine=libaio --direct=1 --gtod_reduce=1 --bs=8k --name=wtest  --invalidate=0 --readwrite=randwrite --iodepth=64 --numjobs=1 --filename=/var/tmp/fiotest --size=10G  --output=test_write.log --rwmixread=0 --rwmixwrite=100  --time_based --runtime=300s
~~~

# Testing Write IOPS...
~~~
fio --randrepeat=0 --verify=0 --ioengine=libaio --direct=1 --gtod_reduce=1 --name=write_iops --filename=/var/tmp/fiotest --bs=4K --iodepth=64 --size=10G --readwrite=randwrite --time_based --ramp_time=2s --runtime=300s
~~~

# Testing Write Bandwidth...
~~~
fio --randrepeat=0 --verify=0 --ioengine=libaio --direct=1 --gtod_reduce=1 --name=write_bw --filename=/var/tmp/fiotest --bs=128K --iodepth=64 --size=10G --readwrite=randwrite --time_based --ramp_time=2s --runtime=300s
~~~

# Testing Write Sequential Speed... <--- duele este 
~~~
fio --randrepeat=0 --verify=0 --ioengine=libaio --direct=1 --gtod_reduce=1 --name=write_seq --filename=/var/tmp/fiotest --bs=1M --iodepth=16 --size=10G --readwrite=write --time_based --ramp_time=2s --runtime=300s --thread --numjobs=4 --offset_increment=500M
~~~
