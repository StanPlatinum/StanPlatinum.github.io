# SGX enclave内部的输出与调试（二）

接上文所说，在我们开发的过程中，很多时候我们需要去调试引入到enclave里的第三方库，也就是需要去调用在Enclave.cpp里定义的printf trampoline。因为在编译的时候我们自己写的Enclave.cpp是需要去链接这些库的，所以这些个第三方库想要再回头去调用我们在Enclave.cpp里定义的函数是不行的。

有人可能会问，为什么打印函数不能定义在enclave外部呢？答案是不行。因为很显然，打印函数就是得在enclave里实现啊，否则就不能在enclave里调用。

这时，就需要引入回调函数来解决这个问题了。回调函数是什么，在这里可以得到一个比较清晰的解答（当然各位也可以搜一搜相关资料，很多地方都有讲）。想要使用回调函数，需要对三个部分分别做改动。

首先是需要加入调试的第三方库里，要传入一个打印函数的函数指针。为了不影响原来的函数，我们可以新增一个函数实现。比如在我的项目中，需要调试capstone里的cs.c里的cs_disasm这个函数，可以重新定义一个带函数指针参数的cs_disasm_dbg函数，像这么写：

https://github.com/StanPlatinum/capstone/blob/master/cs.c#L1049

当然，需要在头文件里加入函数声明：

https://github.com/StanPlatinum/capstone/blob/master/cs_priv.h#L20

第二就是加入调试语句了，比如：

https://github.com/StanPlatinum/capstone/blob/master/cs.c#L1127

这两部做完就可以重新编译第三方库。然后就是进行第三部，在Enclave.cpp里调用这个新的cs_disasm_dbg 。

在调用的时候，填入自己想要的、在上面定义好的打印函数即可。这部分在上一篇文章有讲。

至此，在SGX里对enclave内部进行调试就完成了。至于为什么会想到使用回调函数来做这件事，其实很容易。因为在enclave内部这种场景，就是需要第三方库又调用自己写的代码，这完全符合回调函数的使用模型。这也是为什么回调函数要叫做“回调”的原因。