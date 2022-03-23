---
title: Монтирование файловой системы S3
description: 
published: true
date: 2021-04-08T07:29:44.826Z
tags: 
editor: markdown
dateCreated: 2021-04-06T15:54:13.039Z
---

# Монтирование файловой системы S3
Создаём связку ключей с необходимыми правами:
```
echo <идентификатор ключа>:<секретный ключ> >  ~/.passwd-s3fs
```
```
chmod 600  ~/.passwd-s3fs
```
Разрешаем общее пользование монтируемого раздела путём добавления в /etc/fuse.conf следующей строки:
```
user_allow_other
```
Монтируем бакет:
```
s3fs <имя_бакета> /путь/к/директории -o passwd_file=$HOME/.passwd-s3fs \
    -o url=http://storage.yandexcloud.net -o use_path_request_style -o allow_other
```
Включаем автомонтирование:
```
echo "s3fs#<имя_бакета> /путь/к/директории fuse _netdev,allow_other,use_path_request_style,url=http://storage.yandexcloud.net 0 0" >> /etc/fstab
```
Возможные ключи для оптимизации:

1. **cipher_suites=AESGCM** is only relevant when using an HTTPS endpoint. By default, secure connections to IBM COS use the AES256-SHA cipher suite. Using an AESGCM suite instead greatly reduces the CPU load on your client machine, caused by the TLS crypto functions, while offering the same level of cryptographic security.
2. **kernel_cache** enables the kernel buffer cache on your s3fs mountpoint. This means that objects will only be read once by s3fs, as repetitive reading of the same file can be served from the kernel’s buffer cache. The kernel buffer cache will only use free memory which is not in use by other processes. This option is not recommend if you expect the bucket objects to be overwritten from another process/machine while the bucket is mounted, and your use-case requires live accessing the most up-to-date content.
3. **max_background=1000** improves s3fs concurrent file reading performance. By default, FUSE supports file read requests of up to 128 KB. When asking to read more than that, the kernel split the large request to smaller sub-requests and lets s3fs process them asynchronously. The max_background option sets the global maximum number of such concurrent asynchronous requests. By default, it is set to 12, but setting it to an arbitrary high value (1000) prevents read requests from being blocked, even when reading a large number of files simultaneously.
4. **max_stat_cache_size=100000** reduces the number of redundant HTTP HEAD requests sent by s3fs and reduces the time it takes to list a directory or retrieve file attributes. Typical file system usage makes frequent access to a file’s metadata via a stat() call which maps to HEAD request on the object storage system. By default, s3fs caches the attributes (metadata) of up to 1000 objects. Each cached entry takes up to 0.5 KB of memory. Ideally, you would want the cache to be able to hold the metadata for all of the objects in your bucket. However, you may want to consider the memory usage implications of this caching. Setting it to 100000 will take no more than 0.5 KB * 100000 = 50 MB.
5. **multipart_size=52** will set the maximum size of requests and responses sent and received from the COS server, in MB scale. s3fs sets this to 10 MB by default. Increasing this value also increases the throughput (MB/s) per HTTP connection. On the other hand, the latency for the first byte served from the file will also increase. Therefore, if your use-case only reads a small amount of data from each file, you probably do not want to increase this value. Furthermore, for large objects (say, over 50 MB) throughput increases if this value is small enough to allow the file to be fetched concurrently using multiple requests. I find that the optimal value for this option is around 50 MB. COS best practices suggest using requests that are multiples of 4 MB, and thus the recommendation is to set this option to 52 (MB).
6. **parallel_count=30** sets the maximum number of requests sent concurrently to COS, per single file read/write operation. By default, this is set to 5. For very large objects, you can get more throughput by increasing this value. As with the previous option, keep this value low if you only read a small amount of data of each file.
7. **multireq_max=30** When listing a directory, an object metadata request (HEAD) is sent per each object in the listing (unless the metadata is found in cache). This option limits the number of concurrent such requests sent to COS, for a single directory listing operation. By default it is set to 20. Note that this value must be greater or equal to the parallel_count option above.

