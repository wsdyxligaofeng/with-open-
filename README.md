# with-open-
可以一次性打开多个文件的包，一个文件目录下的多种读取

# 打开文件包的设计
from contextlib import contextmanager


@contextmanager
def open_many(files=None,mode='r'):
#     判断files是否为空，若为空建立空列表
    if files is None:
        files =[]
    try:
#        相当于enter
        fds = []
        for file in files:
            # 将文件写入空白列表中
            fds.append(open(file,mode))
            # 将函数转化成生成器，并且到时候执行命令的时候可以直接进行，若不执行，则再次等待。
            yield fds
    # 若是无法打开文件 则将报出格式错误
    except ValueError as e:
        print ('error:%s' % e)
#     都将执行此项代码,相当于exit
    finally:
        for fd in fds:
            fd.close()
if __name__ == '__main__':
    # 执行程序打开多个文件
    with open_many(['a,txt','b.txt'],'r') as files:
        # 将多个文件放入files，将files中的元素取出，可以执行上述的生成器并且读出。
        for f in files:
            print (f.read())



