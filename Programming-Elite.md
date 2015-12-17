贪心算法
1.最优子结构性质：
当一个问题的最优解中包含其子问题的最优解时，称此问题具有最优子结构性质，也称此结构满足最优性原理。
2. 贪心选择性质：
问题的整体最优解可以通过一些列局部最优的选择来得到。
3. 求解过程

候选集合C：为了构造问题的解决方案，有一个候选集合C作为问题的可能解，即问题的最终解均取自候选集合C。例如，在付款问题中，各种面值的货币构成候选集合。
解集合S：随着贪心的进行，解结合S不断扩展，知道构成一个满足问题的完整解。例如，在付款问题中，已付出的或不构成的集合。
解决函数solution：检查集合S是否构成问题的完整解。例如，在付款问题中，解决函数是已付出的货币金额恰好等于应付款。
选择函数select：即贪心策略，这是贪心算法的关键，它之处哪个候选对象有希望构成问题的解，选择函数通常和目标函数有关。例如，在付款问题中，贪心策略就是在候选集合里面选择面值最大的货币。
可行函数feasible：检查解集合中加入一个候选对象是否可行，即解集合扩展后是否满足约束条件。例如，在付款问题中，可行函数是每一步选择的货币和已付出货币相加不超过应付款。
4.伪代码

    Greedy(C){
        S = {};
        while(not solution(S)){
            x = select(C);
            if(feasible(S, x)){
                S = S+{x};
                C = C-{x};
            }
        }
        return S;
    }
问题1. 付款问题 图问题中的贪心法
问题2. TSP问题 问题3. 图着色问题 问题4. 最小生成树问题 组合问题中的贪心法 问题5. 背包问题 问题6. 活动安排问题 问题7. 多机调度问题

大规模系统设计的问题，比如load balancing, server communication, data consistence等等，而且他会一直深入细节，让你设计一些出错处理什么之类的.
每个任务之间有dependency，怎么安排任务顺序，使得执行任务i的时候，所有被 depend的任务已经执行过了。
用Java设计一个餐馆。有厨师，服务生，客户等等类。设计时我太注意细节了，忘 了考虑多线程。最后在面试管提醒下大致说了一下多线程实现的方案。
udp和tcp的区别，什么时候用tcp，什么时候用udp。tcp是否允许接受重复packet。 cookie是什么在进行操作，一个网站最多有几个cookie。
做一个search engine, 每次搜索到的url肯定会有大量重复。怎么解决?
实现这个search engine, 你的设备是联在一起的100台电脑，它们可以同时工作。 可能整个工作过程的某个时段这台机器得到的url set跟另一台机器得到的url set不一 样，我们又不希望重复劳动。怎么办？
一个非常sparse的matrix，2^64 × 2^64, 设计一个class，内有get(int x, int y ), set(int x, int y, int value)。 用什么数据结构存储它？有哪些选择，各自的 get啊, set的complexity是什么。
Design a class library for writing card games.
然后让我设计一个分布式文件系统，给定 path name，可以读写文件。具体的system design这里就不提了。其中一个细节是，给 定path name，怎么知道哪个node拥有这个文件。我提出需要实现一个lookup function ，它可以是一个hash function，也可以是一个lookup table。如果是lookup table， 为了让所有client sync，可以考虑额外做一个lookup cluster。然后Interviewer很纠 结，既然可以用hash function，为什么还搞得那么复杂。我就告诉他hash function的 缺点。假定一开始有N个node，hash function把M个文件uniformly distribute到N个 node上。某天发现capacity不够，加了一个node。首先，要通知所有的client machine ，configuration 改变了。如果不想重启client ma分治（归并）chine的process，这不是一个 trivial job。其次，文件到node的mapping也变了。比如，本来按照hash function， 一个文件是放在node 1。加了一个node 后，它可能就map到node 2了。平均来说，N/(N +1)的文件需要move到新的node。这个data migration还是很大的。然后我就提出一些 hash function的design，可以减少data migration。
最后他提了一个问题，说要实现一个function，要统计distributed file system所有 目录的大小。前提是，一个目录下的文件可能放在不同的node上。我说这个不就是在每 个node上统计，然后发到一个merge吗。他说对，但是又问用什么data structure来表 示。我说这就是hash table，key就是directory name，value就是大小。因为 directory本身是树结构，这个hash table的key可以用tree来组织。最后让我实现一个 function，把我说得这个data structure serialize成byte array。因为这个byte array就是网络传输的data。我用了depth first traverse。不过等我程序写完，才发 现，用breath first traverse会更方便，code也会很简洁。

他是要我用pthread实现thread pool，以及thread job management。先是 define class interface，然后用pthread的mutex和semaphore实现了consumer/ producer queue。这个queue允许users（producers)加入thread jobs，thread managers(consumers)拿出thread jobs，并执行。

Consider you are constructing a system for data synchronization, what problem will you face, and how you solve it? (I did not do well on this question, since for my understanding, the data synchronization is normally among process, or among different users, like the one in source code version control (Git/repo). I finally understand after 15 mins, he wants to know about multi-threads synchronization.

然后栽在一道large scale的设计题上。绝对不是所有的面试官都让你随意发挥，有的 人心里装了一个答案，问的很模糊，你不答到他那个答案他就是不满意。不知道如何解 决这种情况。大概问答过程如下：

He: how would you design a distributed key-value store Me: DHT or just using clusters He: details? Me: we have a large number of machines. first we use a hash function to retrieve machine ID from the key. Then we connect to the machine and use another hash function to retrieve the address from the key. Then fetch data from that address.

He (seems not satisfied): how much space do you need on the master machine? Me: It depends. If we can use a hash function to derive the IP address of the machine, we don't need extra space. Otherwise, we need a table to store key-IP pairs which is XXX large.

He: say more about how you would get the value on one machine Me: we have two levels of cache, then memory, then disk. We go down to lower levels if we can't retrieve the value on higher levels. (seems like not what he expected)

He: how would you fetch the value on the disk? Please fill in a function char* getData(char *key) { ... } Me: don't know what he asked is different from what I answered. Ask him a lot of questions, but haven't got anything useful

He: Think about what the file system is like Me: Talked about things I know about file systems. Ask him whether he would like me to write that function based on file system or redesign everything.

He: should be based on file systems. Me: go from "/", keep iteratively searching for the current directory using the key, until we hit a file not a directory. Then open that file and read value and return the value.

整个过程，感觉跟他预想的不一样，跟我预想的也不一样。一直觉得key-value pairs 应该是用分布式的no sql的DB来实现的，没想到要去读file。另外自己对于disk读取的 底层API也不了解，所以答题的时候基本凭想象来答，觉得怎样应该算是reasonable的 。这可能是导致杯具的原因。

有两点教训就是。一，不要觉得自己是new grad就可以只写code，答两道数学题，他们 真的什么都考，特别是这种large scale的，什么问题都可以问。二，两个面试之间一 定要take a break，就算不上厕所也要去一趟洗手间让大脑休息一下，我就是到最后两 个有些晕了，没答好杯具了。 DHT B+ tree

固定时间内某网站只允许访问有限次，如何让index次数尽可能的少，又不错过更 新。

Table reservation system. 并行的, 这个用semaphore或mutex tasking的算法 不行么?

Design Patterns
