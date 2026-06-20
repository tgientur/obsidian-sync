# CPU演进史

## 技术起源

CPU（中央处理器）的工程起源可以追溯到1940年代冯·诺依曼架构的提出。1945年冯·诺依曼在EDVAC报告中首次描述了存储程序概念——指令和数据存放在同一存储器中，由处理器顺序取指执行。这一架构成为所有现代CPU的底层蓝图。

1958年TI的Jack Kilby发明集成电路，1971年Intel推出世界上第一款商用微处理器4004，位数仅4位，集成2300个晶体管，主频740kHz，用于Busicom计算器。4004的意义不在于性能，而是证明了"通用可编程处理器"这个概念的可行性——同一颗芯片通过加载不同的软件可以执行完全不同任务，这是CPU作为通用计算平台的基础。

1974年Intel 8080 (8位, 4500晶体管, 2MHz) 成为第一款广泛的微处理器，催生了Altair 8800微型计算机，开启了PC革命。但真正塑造未来40年格局的是1978年的Intel 8086。

## 第一代方案：CISC时代与x86奠基 (1978-1990s)

### 8086与x86指令集架构

1978年Intel推出8086(16位, 29000晶体管, 5-10MHz)，奠定了x86架构。指令集采用了CISC(复杂指令集计算机)设计哲学——提供丰富、变长的指令，让每条指令完成更复杂的工作。8086兼容的8088被IBM选用于1981年的IBM PC，这是历史性的转折点：x86从此绑定了全球最大的计算平台，形成了后来Intel主导的Wintel联盟。

### 80386：32位转折

1985年Intel 80386(275000晶体管, 12-33MHz)实现了32位寻址(4GB内存空间)，引入了保护模式和虚拟8086模式，支持多任务操作系统。这是x86从16位到32位的跨越，为Windows和Linux提供了硬件基础。1989年80486(120万晶体管, 25-50MHz)首次集成L1缓存(8KB)和FPU浮点单元，CPU从单纯的计算核心进化为带缓存的SoC雏形。

### RISC挑战

1980年代，David Patterson和John Hennessy提出RISC理念——简化指令集，每条指令固定长度、单周期执行，让硬件更简单、频率更高。代表产品包括MIPS(R2000, 1986)、SPARC(Sun, 1987)、ARM(Acorn RISC Machine, 1985)。RISC在理论上优于CISC，但因为x86已经有了庞大的软件生态，Intel选择在兼容x86的前提下内部将指令翻译为微操作(micro-ops)，这一思路在Pentium Pro中成熟，延续至今。

## 第二代方案：Pentium时代与超标量架构 (1993-2005)

### Pentium：超标量执行

1993年Intel Pentium(310万晶体管, 60-200MHz)首次在x86中实现超标量执行——两条指令流水线(U-pipe和V-pipe)，一个时钟周期可执行两条指令。同时数据总线从32位拓宽到64位，L1缓存增至16KB。Pentium的名字来自希腊语"pente"(五)，"ium"是元素后缀，代表第五代。

### Pentium Pro与P6架构

1995年Pentium Pro(550万晶体管)引入了三个关键微架构创新：微操作融合(micro-op fusion)、乱序执行(out-of-order execution)、和寄存器重命名(register renaming)。x86复杂指令被翻译成更简单的RISC风格的μops，然后在乱序引擎中调度执行。P6架构是CPU微架构的分水岭——从此CISC处理器在内部以RISC方式工作。

### Pentium 4与NetBurst的教训

2000年Pentium 4引入了NetBurst架构，主攻极高频率(首发1.5GHz, 后期达3.8GHz)。通过超长流水线(20级到31级)提高频率，但每条流水线效率反而下降。NetBurst的高功耗发热问题(热设计功耗高达130W以上)暴露了频率竞赛的物理天花板——这被称为"功耗墙"。Intel被迫放弃NetBurst，回归P6路线并演进到Core架构。

### AMD的竞争与64位转折

2003年AMD推出了AMD64(Athlon 64)，是x86架构从32位到64位的首次扩展，兼容32位x86软件。Intel被迫采纳AMD64标准(x86-64)，至今所有x86处理器都使用AMD的64位指令集扩展。Intel则依靠Hyper-Threading(超线程)技术——让单核模拟双逻辑核——在2000年代初期回击。

## 第三代方案：多核时代 (2005-2020)

### 频率终结与多核转向

2004年Intel取消4GHz的Tejas和Jayhawk计划，标志着频率竞赛彻底结束。物理限制源于三个因素：漏电流随频率指数级增长(CMOS晶体管的亚阈值泄漏)、散热能力达到风冷极限、功耗密度过高。Intel和AMD同时转向多核架构——单个芯片集成多个处理器核心。

### Intel Core架构

2006年Intel Core 2 Duo(Conroe)基于Core微架构(Core 2 Duo E6600)，根源来自以色列海法团队的Pentium M改良路线。与NetBurst相比，Core微架构更短流水线(14级)、更高IPC(每周期指令数)、更低功耗。Core 2在同样频率下性能比Pentium 4高出40%，功耗却低40%。这是Intel历史上最重要的架构转折之一。

### 摩尔定律放缓与Tick-Tock策略

2007年Intel提出Tick-Tock策略：Tick年缩小制程，Tock年更新架构。2007年Penryn(45nm)→2008年Nehalem(架构,CU-DIMM)->2010年Westmere(32nm)->2011年Sandy Bridge(架构,环形总线)->2012年Ivy Bridge(22nm,3D晶体管)->2013年Haswell(架构,AVX2)。但到14nm节点(2014年Broadwell), 摩尔定律明显放缓，Tick-Tock在2016年改为P.A.O.(制程-架构-优化)三阶段，最终延长到每一代四年+周期。

### ARM的崛起

功率墙逼迫整个行业思考高效架构。ARM从移动市场(手机SoC)起步，1990年代设计了低功处理核心，2000年代通过授权模式成为智能手机CPU标准。2010年代ARMv7-A(Cortex-A8/A9)开始进入非手机场景。关键转折出现在：Apple的M1(2020)使用ARM架构实现桌面级性能，功耗仅为同档x86的一半还不到。M1的成功在于Apple对ARM微架构的深度定制——宽发射(8宽解码)、大ROB(重排序缓冲区)、高带宽缓存子系统——直接挑战了Intel在CPU微架构上的领导地位。

### AMD Zen架构逆袭

2017年AMD Zen架构成为AMD重返高性能市场的关键。Zen采用CCX(核心复合体)结构，4核共享L3缓存，通过Infinity Fabric互联。Zen 2(2019, 7nm)首次采用Chiplet设计——将IO Die与计算Die分离，用MCM(多芯片模块)封装。Chiplet设计大幅降低了成本(单个Die良率更高)且允许混合使用不同制程。Zen 3(2020)统一CCX为8核共享32MB L3，IPC提升19%。Zen 4(2022, 5nm)引入AVX-512。Zen 5(2024)继续提升IPC和AI性能。

## 当前主流方案：异构计算与Chiplet (2020s)

### Intel：大小核混合架构

Intel从第12代酷睿(Alder Lake, 2021)开始采用Performance-core(P核)+Efficiency-core(E核)的异构架构。P核(Golden Cove)针对单线程高性能，E核(Gracemont)针对多线程能效。通过Thread Director技术让操作系统智能调度线程到合适核心。Intel 7制程(改名自10nm Enhanced SuperFin)。Raptor Lake(13/14代)简单升级缓存和频率，遭遇了13/14代电压不稳定问题(Vmin Shift缺陷)。

### AMD：Chiplet生态

AMD Zen 4/Zen 5延续Chiplet路线。3D V-Cache技术将额外L3缓存(64MB)堆叠在计算Die上，通过TSV(硅通孔)互联，使游戏性能提升15-20%。Zen 5使用4nm/3nm混合工艺，IPC较Zen 4提升约16%。AMD在数据中心凭借Genoa(SP5平台)和Bergamo(高密度Zen 4c核心)持续扩大服务器市场份额。

### ARM：从终端到服务器

Apple M系列(M1→M2→M3/M4)每代迭代：M1(2020, 5nm, 160亿晶体管)→M2(2022, 增强5nm)→M3(2023, 3nm, 动态缓存)→M4(2024, 3nm增强, AMX AI加速)。Apple的定制核心(宽发射16宽、700+ entry ROB、大量物理寄存器)在单核性能上已经超越Intel和AMD。服务器端：Ampere Computing推出128核Arm处理器(AmpereOne)，AWS Graviton系列(Graviton3E)已大规模部署在云端。NVIDIA Grace CPU(72核Arm, 2023)面向AI/HPC场景。

### Chiplet与UCIe标准

Chiplet(小芯片)是超越摩尔定律的关键路径。AMD Zen 2率先量产，Intel通过EMIB(嵌入式多芯片互连桥接)和Foveros(3D堆叠)跟进。2022年成立的UCIe(Universal Chiplet Interconnect Express)联盟旨在建立开放的Chiplet互连标准，支持AIB(Advanced Interface Bus)和BoW(Bridge of Wires)两种物理层。UCIe将成为未来CPU和SoC设计的基本单元。

## 未来技术路线

### 3D堆叠与异构集成

Hybrid Bonding(混合键合)技术(如Intel Foveros Direct、台积电3D Fabric)实现Die间直接铜对铜键合，间距缩短到微米级。3D堆叠将CPU核心与缓存、I/O、加速器垂直堆叠，大幅降低芯片面积和延迟。

### 新型晶体管结构

Gate-All-Around(GAA)FinFET:台积电N2(2025)和Intel 20A(2024)采用GAAFET，将栅极环绕纳米片四周，比FinFET有更好的通道控制，漏电流更低。CFET(互补FET)：将NMOS和PMOS垂直堆叠，实现标准单元面积减半。这是GAA之后的下一代晶体管结构，预计2030年前后量产。

### 先进封装

玻璃基板(Glass Core Substrate)：相比有机基板，玻璃基板平面度高、热稳定性好、适合更细线路，可支持更大的封装尺寸和更高密度互联。Intel计划在2025年后量产玻璃基板。FOWLP(Fan-Out Wafer Level Package)用于整合AI加速器。

### AI推理与存算一体

CPU内置NPU成为趋势(Intel Meteor Lake的AI引擎、AMD Ryzen AI、Apple Neural Engine)，处理轻量级AI推理任务。存算一体(Computing-in-Memory / Processing-in-Memory)：将计算逻辑嵌入存储器，消除冯·诺依曼瓶颈。三星HBM-PIM和AMD的3D V-Cache都是存算一体化的早期探索。

## 核心瓶颈

### 物理极限

**功耗密度**：晶体管的单位面积功耗并未随制程缩小而等比降低，导致"暗硅"(Dark Silicon)——一个芯片上同时只能运行部分晶体管。**量子隧穿**：当栅极氧化物厚度降到1nm以下，电子会直接隧穿，使晶体管无法彻底关断。**漏电流**：随着制程微缩，亚阈值漏电流和栅极漏电流占比急剧增加。**RC延迟**：互连线延迟随制程缩小，电阻(Cu线截面缩小)和电容(线间距缩小)都在恶化，反超门延迟成为主要瓶颈。

### 经济瓶颈

**先进制程成本**：台积电3nm单片晶圆成本约2万美元，N2(2nm)预计超3万美元。设计一颗5nm芯片的成本超过5亿美元。**EUV光刻机**：ASML的高数值孔径(High-NA) EUV光刻机单价约3.5亿欧元。**小厂退出**：有能力制造最先进制程的只剩下台积电、三星、Intel三家。

### 架构瓶颈

**内存墙**：CPU运算速度增长远快于DRAM访问速度。典型CPU每周期可执行数十条指令，但一次内存访问需要数百周期。多级缓存(L1/L2/L3)只能缓解，无法根本解决。**指令级并行(ILP)极限**：传统超标量架构对指令级并行的挖掘接近物理极限。平均每周期执行的指令数(IPC)在大约4-6条后很难继续提升。

## 产业影响

### 竞争格局

x86阵营：Intel仍然持有PC/服务器CPU最大市场份额，但制程迭代落后(在10nm/7nm节点多次跳票)。AMD凭借台积电先进制程和Chiplet设计，在数据中心份额从2016年的不到1%增长到2024年约30%。ARM阵营：Apple M系列证明了ARM架构能达到甚至超越同代x86性能，推动了ARM在PC和数据中心的全面渗透。RISC-V：作为开源ISA，在某些嵌入式场景(IoT、AI加速器)快速成长，但高性能通用计算中生态积累不足。

### PC市场

Intel年出货CPU约3亿颗，AMD约5000万颗。PC市场2011年达到峰值3.65亿台后进入缓慢萎缩，2023年约2.68亿台。AI PC(集成NPU的个人电脑)成为新增长点，2024年起所有主流CPU厂商(Intel Core Ultra、AMD Ryzen 8040、高通Snapdragon X Elite)将NPU作为标准配置。

### 云数据中心

CPU是云基础设施的核心。AWS、Azure、Google Cloud三大云巨头都在自研CPU：AWS Graviton(ARM)、Azure Cobalt(ARM)、Google Axion(ARM)。自研CPU可优化TCO(总拥有成本)和工作负载匹配。x86在通用计算仍占主导(约90%+)，但ARM架构在云端的份额正在从个位数向两位数攀升。

### 地缘政治

美国对华芯片出口管制(2020年起)限制了中国企业获取先进CPU(E级算力及FinFET制程)。中国CPU厂商走向三条路线：x86生态(海光、兆芯，授权方式)、ARM生态(华为鲲鹏、飞腾)、自研架构(龙芯LoongArch、申威SW64)。国产CPU在党政信创和特定行业(运营商、金融)有可观部署，但在通用PC和高端服务器市场与Intel/AMD仍有较大差距。

## 参考文献

- Intel Microprocessor Quick Reference Guide (Intel 2023)
- Hennessy & Patterson, Computer Architecture: A Quantitative Approach, 6th Ed.
- AMD Zen Microarchitecture White Papers (AMD 2017-2024)
- Apple M1 / M3 Microarchitecture (AnandTech, WikiChip Fuse)
- UCIe Specification 1.1 (UCIe Consortium 2024)
- 龙芯中科产品白皮书 (2024)
- ASML High-NA EUV技术路线图
- 国际半导体产业协会(SEMI)年度报告
