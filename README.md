# mips-cpu

支持指令：lw, sw, beq, bne, addi, add, sub, and, or, slt, j, addi, ori, andi

`\framework` 为一个支持 lw, sw, beq, bne, addi, add, sub, and, or, slt 的框架，只需要当做填空题补全等号后面的内容即可。

众所周知，MIPS体系的五级流水线CPU分为五个阶段：取指（IF）、译码（ID）、执行（EX）、存储器（MEM）、写回（WB）。所以这根本算不上“设计”，基本上就是把这个图翻译一遍：
![在这里插入图片描述](https://img-blog.csdnimg.cn/cc83a6b26a484e24b09192079fc30fad.png#pic_center)
可以发现节点数比较多，为了减少 debug 的时间，程序的框架就显得非常重要了。
这里采用了按阶段划分的方式，每个阶段作为一个模块写在一个 .sv 里，然后ALU、内存、寄存器文件又单列出来，最后在顶层处理模块间的连线和冲突模块的设计。

举例来说，如下图，执行阶段（EX.sv）描述绿框内的电路，这个模块的输入输出就是所有与绿框有交的节点，输入大致为蓝线上的节点，输出大致为红线上的节点。这里的大致是因为还有一些需要用到的节点是不在这里面的（比如解决冲突连出来的）。
![请添加图片描述](https://img-blog.csdnimg.cn/ef639f99105a435faed23eb6ed4cabd5.png)

这样做的好处是每个模块独立性较强，几十行代码即可解决，静态查错比较方便；模块间连接也比较规范，很好处理。坏处就是我在赌过了 simulation 后能一遍上板子，还好赌赢了。
