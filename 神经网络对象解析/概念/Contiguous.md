直接含义是,连续,连续的.

在pytorch中的含义是,该对象指代的tensor是否在内存上是连续的.
在pytorch中,有两个相关的api:is_contiguous和contiguous().
即,判断该对象是否是连续的,和将该对象连续化.

调用的接口没有写文档,再说吧.