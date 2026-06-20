# HBM演进史

## 技术起源

HBM（High Bandwidth Memory，高带宽存储器）的起源要追溯到存储带宽瓶颈的爆发。2010年前后，GPU的着色器数量快速增长，但对显存带宽的需求增速更快——GDDR5虽然将频率推到了 GHz 级别，但其128位或256位的内存总线需要通过大量PCB走线来实现宽位宽，功耗和面积代价越来越高。传统 DRAM 的架构决定了它的位宽扩展受限于封装引脚数和信号完整性。AMD、SK海力士和JEDEC在2010-2013年间联合推动了一种新思路：将多个DRAM die通过硅中介层（Silicon Interposer）垂直堆叠，并用TSV（Through Silicon Via，硅通孔）穿透各层实现超短互联，把总线宽度从128位提升到1024位甚至更宽。这个方案就是HBM。

HBM的核心哲学是用"更短更宽"的总线取代"更长更窄"的总线。传统GDDR的走线距离长达数厘米，信号需要驱动通过PCB走线；而HBM通过TSV和微凸块（Micro Bumps）将die之间的通信距离缩短到几十微米，功耗大幅降低。同时，1024位的总线宽度意味着即使运行在相对较低的频率（如1GHz），理论带宽也能轻松突破256GB/s。这项技术从一开始就是为高性能计算和图形处理设计的。

代表开发方是SK海力士和AMD。AMD的Fiji GPU是第一个搭载HBM的商业产品，这比NVIDIA更早选择HBM路线。

## 第一代方案

### HBM1（2013-2015）

HBM1是JEDEC标准化的第一代产品，2013年亮相，2015年随AMD Fury系列显卡进入消费市场。

| 参数 | HBM1 |
|---|---|
| 每堆叠带宽 | 128 GB/s |
| 最大堆叠层数 | 4层（4-Hi） |
| 每层容量 | 1 Gb（128 MB） |
| 总容量（单堆叠） | 512 MB（4Hi × 128MB） |
| 总线宽度 | 1024位 |
| I/O电压 | 1.3V |
| 工艺节点 | 20nm级 |
| 封装方式 | 2.5D硅中介层 |

HBM1的1024位总线在当时是一颗震撼弹。以AMD Radeon R9 Fury X为例，它搭载4颗HBM堆叠（共4GB），系统带宽达到512GB/s，远超同期NVIDIA GTX 980 Ti搭载GDDR5的336GB/s。代价是封装复杂度飙高——硅中介层本身就是一块昂贵的硅片，需要在上面走通数以万计的微互联。Fury X的四颗HBM与GPU核心一起放在中介层上，经过CoWoS（Chip-on-Wafer-on-Substrate）工艺完成封装。

核心影响是：HBM1证明了从架构到封装的全链条可行性。但它容量太小（单堆叠上限512MB），成本太高，仅限于顶级旗舰显卡和专业加速卡。

### GDDR5的同期反击

NVIDIA在同期选择继续深耕GDDR5。GTX 980 Ti使用384位GDDR5接口达到336GB/s，虽不及HBM1的512GB/s，但成本优势巨大，且单颗GDDR5容量可达4Gb，轻松做到6GB显存。HBM1在消费级市场叫好不叫座。

但HBM的功耗优势是实打实的——HBM1功耗约是其带宽的四分之一，而GDDR5要达到同等带宽需要功耗翻倍。这为HBM在后来的AI数据中心崛起埋下了伏笔。

## 第二代方案

### HBM2（2016-2018）

HBM2于2016年由JEDEC标准化，大幅提升了容量和带宽上限。SK海力士、三星、美光三家同时进入量产。

| 参数 | HBM2 |
|---|---|
| 每堆叠带宽 | 256 GB/s |
| 最大堆叠层数 | 8层（8-Hi） |
| 每层容量 | 2 Gb（256 MB）→ 8 Gb（1 GB） |
| 总容量（单堆叠） | 最高8 GB（8Hi × 1GB） |
| 总线宽度 | 1024位 |
| I/O电压 | 1.2V |
| 最大带宽 | 307 GB/s @ 2.4 Gbps |
| 工艺节点 | 18-20nm |

HBM2有两个关键进步。第一是层数从4堆叠翻倍到8堆叠，容量显著提升。第二是每die容量从1Gb进化到8Gb，单堆叠最大8GB。这使得NVIDIA的V100（2017年）能够搭载16GB HBM2，提供900GB/s的惊人带宽。

V100成为AI训练的事实标准。在ImageNet训练任务上，V100搭配HBM2的带宽优势让大批量训练（large batch training）成为可能，直接推动了Batch Normalization和分布式训练的实用化。NVIDIA自此锁定了HBM路线。

HBM2的封装也经历了迭代。从早期CoWoS的"硅中介层"方案，逐渐探索了更经济的方式。NVIDIA在V100（GV100核心）上使用CoWoS-S（silicon interposer），堆叠4颗HBM2裸片。

### HBM2e（2019-2020）

HBM2e是HBM2的提升版本，本质上是通过提升I/O速率和die密度来延长HBM2的生命周期。

| 参数 | HBM2e |
|---|---|
| 每堆叠带宽 | 约 410 GB/s |
| 最大容量（单堆叠） | 16 GB（8Hi × 2GB） |
| I/O速率 | 3.2 Gbps |
| 工艺节点 | 1z nm |

NVIDIA的A100（2020年）搭载了80GB HBM2e（5堆叠×16GB），峰值带宽2 TB/s。AMD的MI250X也采用HBM2e，单颗容量达到128GB（8堆叠×16GB）。这一代的驱动力完全是AI训练需求。BERT、GPT-2等大规模Transformer模型的参数量从数亿增长到数十亿，模型参数和中间激活值的存储需求暴涨，HBM2e的带宽和容量是支撑增长的基石。

值得注意的是HBM2e也是国产替代的重要参考节点。长鑫存储（CXMT）在HBM2e领域展开跟踪研发。

## 第三代方案

### HBM3（2022-2023）

HBM3是JEDEC发布的新一代标准，核心目标是突破1TB/s单堆叠带宽。

| 参数 | HBM3 |
|---|---|
| 每堆叠带宽 | 约 819 GB/s |
| 最大堆叠层数 | 12层（12-Hi） |
| 每层容量 | 16 Gb（2 GB）→ 24 Gb（3 GB） |
| 总容量（单堆叠） | 24 GB（12Hi × 2GB） |
| I/O速率 | 6.4 Gbps |
| I/O电压 | 1.1V |
| 工艺节点 | 1β nm（SK海力士） |

HBM3引入了多个新特性。最关键的是伪通道（Pseudo Channel）架构拆分，将1024位分成8个128位伪通道，每个通道可独立操作，降低访问粒度、提升利用率。同时HBM3支持ECC（Error Correction Code）纠错，整体可靠性提高。在AI训练场景下，HBM3的比特错误率（BER）显著低于HBM2e。

NVIDIA H100搭载了6堆叠12-Hi HBM3共80GB（另有H100 NVL的188GB版本），总带宽3.35 TB/s，比A100的2 TB/s提升67%。这意味着H100处理LLaMA-65B模型时，参数带宽瓶颈大幅缓解。

SK海力士在HBM3领域的领先地位确立。其采用MR-MUF（Mass Reflow Molded Underfill）工艺替代传统NCF（Non-Conductive Film）工艺，在堆叠翘曲控制和散热上表现更好。

### HBM3e（2024）

HBM3e（又称HBM3 Enhanced或HBM3 Gen2）是2024年的主要量产版本，进一步提高速率和容量。

| 参数 | HBM3e |
|---|---|
| 每堆叠带宽 | 约 1.2-1.6 TB/s |
| 最大堆叠层数 | 12层 → 实验性16层 |
| 每层容量 | 24 Gb（3 GB）→ 36 Gb |
| 总容量（单堆叠） | 36 GB |
| I/O速率 | 9.2-12.8 Gbps |
| 封装工艺 | 混合键合（Hybrid Bonding）探索 |

HBM3e是推动2024-2025年AI算力爆炸的关键存储技术。NVIDIA H200（2024年发布）率先搭载HBM3e，单堆叠12-Hi提供约1.15 TB/s带宽，总容量提升至141GB（比H100的80GB多76%）。H200在LLaMA-70B推理任务中比H100快1.9倍，主因就是HBM3e容量翻倍减少了显存溢出。

AMD MI300X则搭载192GB HBM3e，带宽达到5.2 TB/s。SK海力士在HBM3e量产竞赛中领先三星，率先通过NVIDIA质量认证。三星还引入了HCB（Hybrid Copper Bonding）技术尝试在HBM3e中实现更薄、更高效的堆叠。

## 当前主流方案

截至2024-2025年，HBM3e是AI GPU和加速卡的绝对主流方案。

- **NVIDIA H200**：6堆叠 HBM3e，141GB，约7 TB/s
- **NVIDIA B200（Blackwell）**：8堆叠 HBM3e，192GB，约8-9 TB/s（采用CoWoS-L先进封装）
- **AMD MI300X**：8堆叠 HBM3e，192GB，5.2 TB/s
- **Intel Gaudi 3**：部分采用HBM2e/HBM3混合方案

量产主力供应商是SK海力士（约50%+份额）、三星（约35%）、美光（约10-15%）。三家都在加速推进HBM3e产能，2025年全球HBM产能预计较2023年增长4-5倍。

封装层面，CoWoS-S（硅中介层）仍是主流，但台积电CoWoS-L（局部硅互联）在Blackwell上应用，提升了I/O密度和信号完整性。三星则发展I-Cube和X-Cube封装方案。

系统层面的挑战是散热。HBM堆叠功耗从HBM1的约5W/堆叠增长到HBM3e的15-20W/堆叠，8堆叠的总功耗达120-160W。液冷方案在高密度AI服务器中成为必备。

## 未来技术路线

### HBM4（2026预期）

HBM4是JEDEC正在标准化的下一代方案，预计2026年进入量产。关键在于架构层面的变革。

| 参数 | HBM4（预期） |
|---|---|
| 每堆叠带宽 | 2-2.5 TB/s |
| 最大堆叠层数 | 16层（16-Hi） |
| I/O速率 | 16-24 Gbps |
| 总线宽度 | 2048位（HBM4e进一步扩展） |
| 每层容量 | 32-64 Gb（4-8 GB）|
| 封装方式 | 混合键合（Hybrid Bonding）为主 |

HBM4最核心的变化是总线宽度从1024位翻倍到2048位。这意味着封装复杂度大幅提高。JEDEC还有一个HBM4e规格，可能进一步扩展到3072位甚至4096位。这样做的目的是在不大幅增加I/O速率（避免信号完整性恶化）的前提下，通过更宽的总线实现带宽倍增。

另一个趋势是异构集成。HBM4堆叠中可能包含Base die（基础逻辑层），承担部分内存控制功能。当前HBM的控制逻辑在GPU核心侧，未来部分功能下沉到HBM堆叠内部，可以降低GPU核心复杂度，缩短访问路径。SK海力士和三星都在研发HBM4的custom die方案。

工艺方面，HBM4将采用混合键合（Hybrid Bonding）替代微凸块（Micro Bump）。混合键合消除了凸块层，将上下die的铜垫直接键合，互联间距从40-50μm缩小到10μm以内，密度提升10倍以上，功耗降低40%。

### 3D DRAM和存储级内存

更远期（2028+）的技术路线集中在两点。一个是3D DRAM，借鉴3D NAND的垂直堆叠思路，在单一die内实现多层存储单元堆叠。三星正在开发3D DRAM原型，目标是将单die容量提升到64-128Gb。另一个方向是计算存储融合，在HBM堆叠中加入近存计算逻辑（Processing near Memory），减少数据搬运能耗。三星的HBM-PIM已经在HBM2e上展示过初步原型。

## 核心瓶颈

**封装良率**：HBM的2.5D/3D封装是一大瓶颈。每增加一层堆叠，封装良率以复杂度指数下降。12层到16层堆叠的跨越，对键合精度、晶圆翘曲控制、热应力管理提出了极高要求。

**散热**：HBM3e单堆叠功耗已达15-20W，8堆叠下120-160W的总功耗必须依赖液冷。HBM4的2048位总线带来的额外功耗可能使堆叠功耗突破25W。散热瓶颈将制约AI服务器的部署密度。

**互连瓶颈**：HBM和GPU之间的物理互连速率受到硅中介层布线能力限制。当带宽需求从TB/s迈向10TB/s级别，传统CoWoS-S的硅通孔密度和介电损耗会成为天花板。CoWoS-L和混合键合是应对方案，但成本大幅上升。

**产能限制**：HBM需要同时消化先进DRAM晶圆和先进封装产能。2024-2025年间全球HBM产能大幅扩张，但高端DRAM产能（1α/1βnm节点）有限，供需持续紧张。

**内存墙（Memory Wall）**：GPU计算能力（FLOPS）的增长快于HBM带宽增长，导致单位FLOPS可获取的带宽逐年下降。H100的HBM带宽约3.35TB/s对应989TFLOPS（FP8），每个TFLOPS仅约3.4GB/s带宽；而B200的带宽约8TB/s对应4500TFLOPS（FP8），每个TFLOPS仅约1.8GB/s。如果带宽增长率无法跟上算力增长，模型训练效率将受内存墙制约。

## 产业影响

HBM技术的演进直接影响了AI芯片的竞争格局。

**GPU旗舰的门槛**：HBM的容量和带宽已经成为AI GPU的代际区分标志。从V100（16GB HBM2）到H100（80GB HBM3）到B200（192GB HBM3e），每一代GPU的竞争力都高度绑定HBM配置。NVIDIA产品节奏甚至需要根据SK海力士和三星的HBM产能排期来制定。

**内存厂商的战略重心转移**：SK海力士、三星和美光的高利润来源从手机DRAM转向HBM。HBM的毛利率远高于标准DRAM（2024年约40-50% vs 传统DRAM的10-20%），驱动三家巨头将DRAM资本支出的30%以上投向HBM相关产能。

**先进封装产业链崛起**：台积电的CoWoS产能成为AI芯片供应的瓶颈之一。CoWoS产能从2022年的约2万片/月扩产到2024年的约5-6万片/月，仍供不应求。日月光、Amkor、长电科技等封装厂商也在快速建设2.5D/3D封装能力。

**国产替代的追赶**：中国DRAM制造商长鑫存储（CXMT）在HBM领域的进度受设备制裁限制。HBM需要使用混合键合机、TSV刻蚀机、临时键合/解键合机等先进设备，其中多数受瓦森纳协定管制。国产设备在HBM2e级别有所突破，但在HBM3/HBM4所需的高精度工艺上仍有差距。

**行业标准的话语权**：HBM标准从JEDEC主导逐步转向JEDEC与客户联合定义并行的模式。NVIDIA、AMD、Intel等大客户正在推动HBM4的定制化，标准从"通用存储"向"半定制AI存储"演进，这改变了存储厂和芯片厂之间的合作关系。

## 参考文献

[1] JEDEC. JESD235: High Bandwidth Memory (HBM) DRAM[S]. 2013.
[2] JEDEC. JESD235A: High Bandwidth Memory 2 (HBM2) DRAM[S]. 2016.
[3] JEDEC. JESD235C: High Bandwidth Memory 3 (HBM3) DRAM[S]. 2022.
[4] AMD. AMD Radeon R9 Fury X Whitepaper[R]. 2015.
[5] NVIDIA. NVIDIA Tesla V100 GPU Architecture Whitepaper[R]. 2017.
[6] NVIDIA. NVIDIA A100 Tensor Core GPU Architecture Whitepaper[R]. 2020.
[7] NVIDIA. NVIDIA H100 Tensor Core GPU Architecture Whitepaper[R]. 2022.
[8] SK海力士. HBM3e: The Next Generation High Bandwidth Memory[R]. 2024.
[9] KANG U, CHUNG H, HEO S, et al. 8 Gb 3D DDR3 DRAM using through-silicon-via technology[C]. ISSCC, 2009.
[10] LEE J, KIM J, et al. A 1.2V 64Gb 8-Hi 3D-stacked DRAM with 8.2Gb/s/pin HBM3 interface[C]. ISSCC, 2023.
[11] 三星电子. HBM-PIM: Processing-in-Memory for AI Acceleration[R]. 2023.
[12] 台积电. CoWoS-S and CoWoS-L Packaging Technologies[R]. 2024.
[13] WILLIAMS S, WATERMAN A, PATTERSON D. Roofline: an insightful visual performance model for multicore architectures[J]. CACM, 2009, 52(4): 65-76.
