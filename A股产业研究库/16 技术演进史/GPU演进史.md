# GPU演进史

## 技术起源（1990s）

1990年代初期，3D图形渲染的兴起对计算能力提出了前所未有的要求。实时3D图形涉及两大核心计算任务：**几何变换（Transform）和光照计算（Lighting）**，即T&L。每一项都需要对海量顶点执行4x4矩阵乘法运算——一个简单的3D场景动辄包含数万个顶点，每个顶点需要多次矩阵运算才能完成从模型坐标系到屏幕坐标系的投影变换。

当时的CPU串行架构处理这类高度并行的向量运算效率极低。以1996年发布的Voodoo Graphics（3dfx）为代表的第一代3D加速卡只能处理像素渲染（光栅化），几何变换仍由CPU承担。每生成一帧画面，CPU需要先计算所有顶点的变换和光照，再将数据交给显卡绘制像素。当场景复杂度增加，CPU成为瓶颈，帧率急剧下降。

这一矛盾的根源在于**CPU设计哲学**与**3D图形计算本质**之间的错配。CPU为通用串行计算优化——低延迟、强分支预测、大缓存；而3D图形计算本质上是**大规模数据并行**——数千个顶点执行相同的矩阵运算，彼此无依赖关系，天然适合SIMD（单指令多数据流）架构。专用硬件取代CPU处理这些并行计算只是时间问题。

1999年，NVIDIA GeForce 256正式发布，NVIDIA首次将其称为"GPU"（Graphics Processing Unit）[1]，标志着一个独立计算单元品类的诞生。背后的关键洞察是：**将T&L管线从CPU搬到专用硬件，释放3D场景复杂度的增长天花板。**

## 第一代：固定管线GPU（1995-2005）

**核心设计：固定功能流水线（Fixed-Function Pipeline）**

此阶段的GPU内部结构是一条固定顺序的处理流水线：顶点输入 → 变换与光照（T&L）→ 光栅化 → 纹理映射 → 像素输出。每个阶段由专用硬件模块完成，管线顺序不可更改，计算逻辑不可编程。开发者只能通过调整参数（光照方向、纹理坐标、材质颜色）控制渲染结果，无法自定义顶点或像素的处理算法。

**关键代表产品：**

- **3dfx Voodoo（1996）**：首个获得广泛商业成功的3D加速卡，仅处理像素渲染，T&L仍需CPU完成。开放GL驱动支持不足，依赖Glide专用API。
- **NVIDIA GeForce 256（1999）**：第一款集成硬件T&L的消费级GPU，支持DirectX 7。将顶点变换从CPU移至GPU，帧率在复杂场景下提升2-3倍。
- **ATI Radeon 9700（2002）**：支持DirectX 9，首次引入浮点像素着色器（Pixel Shader 2.0），兼容OpenGL 2.0。在《Doom 3》和《Half-Life 2》为代表的高画质游戏驱动下大规模普及。

**技术局限性：**

固定管线GPU的处理能力受限于硬件预设的算法。开发者无法实现环境光遮蔽（Ambient Occlusion）、次表面散射（Subsurface Scattering）、动态全局光照等技术——这些都需要自定义像素处理逻辑。开发者只能通过"技巧串接"（将多个渲染Pass组合）模拟效果，效率低、灵活性差。

与此同时，GPU的并行计算特性开始被少数研究者注意。2002年，斯坦福大学的Ian Buck等人利用OpenGL和Cg语言在GeForce FX上实现了稀疏矩阵乘法[2]，但需要通过图形API伪装计算——将数据编码为纹理、将算法编码为着色程序——开发体验极差。

**产业格局：**

这一时期GPU市场经历剧烈洗牌。3dfx于2000年被NVIDIA收购，S3 Graphics逐渐边缘化，Matrox退守专业领域。NVIDIA与ATI（2006年被AMD收购）形成双寡头格局，围绕DirectX版本的升级展开激烈竞争，每18个月推出一代新架构。

## 第二代：可编程着色器GPU（2005-2012）

**核心突破：统一着色器架构 + GPU通用计算**

固定管线的根本问题在于：顶点着色器和像素着色器是两套隔离的硬件资源。当场景多边形密集时，顶点着色器满载而像素着色器空闲；当场景纹理复杂时情况则相反。资源利用率低下。

2006年11月，NVIDIA发布G80架构（GeForce 8800 GTX）[3]，核心设计是**统一着色器（Unified Shader）**：所有着色器单元可以动态分配为顶点处理或像素处理，硬件利用率大幅提升。G80也是首个支持C语言的GPU——开发者可以通过CUDA（Compute Unified Device Architecture，2007发布）直接编写GPU程序，无需伪装成图形API。

**关键代表产品：**

- **NVIDIA GeForce 8800 GTX（2006，G80架构）**：128个统一流处理器（Stream Processor），384-bit显存位宽，支持DirectX 10。浮点性能约345 GFLOPS。
- **AMD Radeon HD 4870（2008，RV770架构）**：800个流处理器，首次在GPU上实现1 TFLOPS浮点性能，定价仅299美元，推动了通用计算GPU的普及成本下降。
- **NVIDIA GeForce GTX 280（2008，GT200架构）**：240个流处理器，1GB显存，1 TFLOPS单精度浮点性能，CUDA生态逐渐成熟。

**GPGPU概念的诞生与产业影响：**

CUDA的发布将GPU从一个图形芯片转变为**可编程的通用并行计算设备**。这是GPU历史上最重要的范式转换之一。

CUDA的核心创新在于：将GPU的组织方式从"图形流水线"抽象为**线程层次模型（Thread Hierarchy）**。开发者将计算任务划分为Grid → Block → Thread三层，由硬件调度器自动将Block分配到流多处理器（SM）上执行。这种抽象隐藏了GPU内部物理资源的细节，使通用计算开发者无需理解图形渲染即可使用GPU算力。

首批使用CUDA的非图形应用领域包括：
- **分子动力学模拟**：GROMACS和NAMD利用GPU加速分子力学计算，加速比达10-50倍
- **地震数据处理**：油气勘探中的逆时偏移（RTM）算法通过GPU在数小时内完成CPU数天的计算
- **医学成像**：CT图像重建的迭代算法（如CGLS）在GPU上获得10倍以上加速

2009年，OpenCL标准发布（苹果发起、Khronos管理），旨在提供跨平台（AMD GPU、Intel CPU、ARM）的异构计算框架，但生态成熟度和开发者体验始终落后于CUDA，并未动摇NVIDIA在GPGPU领域的主导地位。

**这一阶段的底层逻辑：** 当一件工作可以被分解为数千个独立子任务时，GPU的大规模并行架构天然优于CPU的串行优化设计。GPU的核心优势不在于单核性能，而在于**并行规模（Parallel Throughput）**。

## 第三代：通用计算GPU（2012-2020）

这一阶段GPU的发展由两条主线驱动：传统图形渲染的持续升级，以及深度学习在2012年ImageNet竞赛中爆发的算力需求。

**关键架构演进：**

- **Kepler（2012，NVIDIA GK104/GK110）**：引入Dynamic Parallelism（GPU线程可自行启动子核函数，减少CPU-GPU通信延迟），Hyper-Q（多个CPU线程共享一条GPU命令队列），以及Grid Management Unit（硬件级核函数调度）。GK110拥有2880个CUDA核心，双精度性能达1.3 TFLOPS。
- **Maxwell（2014，GM200）**：重新设计SM（流多处理器）调度单元，功耗效率比Kepler提升2倍。引入Maxwell动态并行技术，改进了GPU任务分发效率。首次将深度学习推理引入消费级GPU。
- **Pascal（2016，GP100）**：引入NVLink（第一代，80GB/s双向带宽），统一内存（Unified Memory，CPU-GPU共享地址空间），以及16nm FinFET工艺。Tesla P100单精度浮点性能达9.3 TFLOPS。NVLink的出现解决了一个根本矛盾：PCIe 3.0 x16的单向带宽仅16GB/s，远无法匹配GPU的显存带宽和处理能力——多GPU协同时，PCIe成为瓶颈。
- **Volta（2017，GV100）**：**最重要的架构设计转折点。** 首次引入Tensor Core——专为矩阵乘加运算（D = A × B + C）设计的专用执行单元。每个Tensor Core每时钟周期可执行4×4×4矩阵运算，Volta V100拥有640个Tensor Core，深度学习训练吞吐比Pascal提升12倍[4]。同时引入多进程服务MPS优化（改进版）和独立线程调度（Independent Thread Scheduling）。

**Tensor Core的意义：**

传统CUDA Core执行矩阵乘法需要多次FMA（融合乘加）指令，效率受限于指令发射宽度。Tensor Core是**从硬件层面直接实现矩阵乘加运算**，在单个周期内完成完整的4×4矩阵运算。这是GPU从"通用并行处理器"向**AI专用加速器**过渡的分水岭。没有Tensor Core，现代大语言模型的训练成本将高出至少一个数量级。

**产业影响：**

- 2012年，AlexNet使用两块GTX 580训练了6天，以15.3%的Top-5错误率赢得ImageNet竞赛，比第二名低10.8个百分点[5]。业界首次认识到GPU是深度学习的核心加速器。
- 2016年，Google发布TPU v1（张量处理单元），为TensorFlow定制设计，标志着互联网巨头开始自研AI芯片。TPU v1仅支持推理（INT8），峰值性能达92 TOPS，但无法用于训练。这也揭示了一个趋势：**GPU并非AI计算的唯一答案，但它是生态最完善、最灵活的通用AI计算平台。**
- 2018年，BERT模型在64块V100上训练了4天，参数量3.4亿。GPU集群成为AI实验室的基础设施。
- 2019年，GPT-2（15亿参数）在V100集群上训练，NVIDIA数据中心业务收入达到29.7亿美元，首次被列为单独报告分部。

**竞争动态：** AMD在这一阶段先后发布Fiji（2015）、Vega（2017）和RDNA（2019）架构，硬件浮点性能不逊色于NVIDIA同期产品，但ROCm软件生态远弱于CUDA，在AI训练领域几乎没有市场份额。

## 第四代：AI专用GPU（2020-至今）

2020年5月，NVIDIA发布Ampere A100，标志着GPU进入**AI专用化**阶段。与上一代产品不同，A100的设计目标不再以"兼顾图形和计算"为优先，而是**围绕AI训练和推理的工作负载模式重构芯片架构**。

**核心产品与突破：**

- **A100（2020，GA100，Ampere架构）**：540亿晶体管，7nm工艺，826mm² die size（接近掩模版极限）。引入第三代Tensor Core，支持TF32格式（19.5 TFLOPS）、FP16/BF16（312 TFLOPS）和INT8（624 TOPS）。MIG（Multi-Instance GPU）允许将一块A100物理划分为最多7个独立GPU实例，每个实例拥有独立显存和缓存，用于多租户云环境。A100的显存带宽达2.0 TB/s（HBM2e）[6]。单卡AI训练吞吐比V100提升约6倍（BF16）。
- **H100（2022，GH100，Hopper架构）**：800亿晶体管，4nm定制工艺（TSMC 4N），单精度Tensor Core性能达2000 TFLOPS（FP8）。核心创新**Transformer Engine**：动态检测网络中每层的激活值分布，在FP8和FP16之间自适应切换精度，使Transformer类模型训练速度提升约9倍（相比A100）[7]。引入NVLink Switch互联系统：通过NVSwitch芯片，最多256个H100连接为单一计算域，GPU间带宽达900 GB/s（NVLink 4.0，每个GPU 18个链路）。引入DPX指令集加速动态规划算法（如路径规划、基因组比对）。
- **B200（2024，GB200，Blackwell架构）**：2080亿晶体管，通过**Two-Die CoWoS封装**（两个Die通过桥接芯片互联）突破单芯片面积限制。每块GB200包含两个GPU Die，单精度Tensor Core性能达45000 TFLOPS（FP4）。引入第二代Transformer Engine（支持FP4和FP6精度[8]）、第五代NVLink（1800 GB/s带宽）、以及可靠性引擎（计算错误检测和恢复）。

**产业影响的底层逻辑：**

Transformer模型的规模增长速度（每年4-10倍参数翻倍）持续超越摩尔定律（每2年2倍晶体管密度）。这意味着**算力需求按照超摩尔定律增长**，完全依赖单芯片性能提升无法满足，必须通过系统级创新解决：

1. **精度收缩（Precision Scaling）**：FP32 → TF32 → FP16/BF16 → FP8 → FP4/FP6。每次精度降级在保证模型质量的前提下带来2倍左右的算力密度提升。
2. **互联带宽（NVLink 2/3/4/5代**：从160GB/s（P100）增长到1800GB/s（B200），多GPU训练的效率瓶颈从计算转向通信。
3. **机柜级系统**：DGX/NVIDIA MGX将GPU、NVSwitch、InfiniBand/NIC集成到标准化机柜，降低数据中心部署门槛。

一个关键的结构性变化：**2023财年，NVIDIA数据中心收入（150亿美元）首次超过游戏收入（90亿美元）**[9]，GPU的第一大市场已从游戏转为AI计算。

**竞争格局的演进：**

- **AMD MI300X（2023）**：基于CDNA 3架构，整合8个GCD（Graphics Compute Die）+ 4个IOD（I/O Die），拥有192GB HBM3显存和5.3TB/s带宽。硬件参数与H100相当，但软件生态差距使实际AI训练效率约低30-50%。
- **Intel Gaudi 3（2024）**：Habana Labs设计的AI加速器，面向AI训练，但市场份额微小（<2%）。
- **AI ASIC挑战者**：Google TPU v5p（2023，单Pod 8960芯片，100+ ExaFLOPS算力）、Amazon Trainium 2（2024）、Microsoft Maia 100（2024）。这些专用芯片在特定模型（自家生态内）的性价比可能优于GPU，但通用性和用户基数的差距意味着它们无法威胁GPU在AI算力的核心地位。

## 当前主流方案

截至2026年，GPU选型已高度分化：

| 类型 | 代表产品 | 主要场景 | 关键参数 | 典型价格 |
|------|----------|----------|----------|----------|
| 训练卡 | H100 SXM / B200 | 大模型预训练、SFT、RLHF | H100: 2000 TFLOPS(FP8), 80GB HBM3; B200: 45000 TFLOPS(FP4), 192GB HBM3e | H100: ~$30k; B200: ~$70k |
| 推理卡 | L40S / H200 NVL | 在线推理、RAG、AI Agent部署 | H200: 141GB HBM3e, 4.8TB/s带宽 | L40S: ~$8k; H200: ~$35k |
| 消费级 | RTX 4090 / RTX 5090 | 个人AI开发、本地微调、小模型推理 | RTX 5090: 32GB GDDR7, 3350 AI TOPS(FP4) | RTX 5090: ~$1999 |

消费级GPU经常被低估。RTX 4090的AI算力（1321 TFLOPS FP8）已超过V100（125 TFLOPS FP16），价格仅为H100的约1/15。个人开发者和小型实验室可以通过多卡RTX方案以极低成本搭建AI训练环境。

## 未来技术路线

**NVIDIA Rubin架构（预计2026-2027）**：

根据NVIDIA GPU技术大会透露信息[10]，Rubin架构将采用NVIDIA定制3nm工艺，引入HBM4显存（预计带宽6.4-8.0 TB/s），第六代NVLink（3600 GB/s+双向带宽），并支持新引脚标准以适配更高密度互联。晶体管数量预计达到2500亿级别，通过多Die设计（超过黑威尔的2Die）实现。

**Chiplet设计的趋势路径**：

传统单芯片GPU面临Retical Limit（光刻掩模版极限约858mm²）。AMD已经率先采用Chiplet路线——MI300X使用8个CDNA 3 Die + 4个I/O Die，13个小芯片（Chiplet）通过Infinity Fabric互联。NVIDIA在Blackwell上首次使用双Die封装，Rubin预计采用更激进的Multi-Die设计。Chiplet路线的核心挑战是**Die间互联的带宽**和**热管理**，NVIDIA的NVLink-C2C和AMD的Infinity Architecture是当前两种主要方案。

**液冷成为标配**：

B200的单卡功耗高达1000-1200W（双Die），H100约700W。传统风冷已无法满足散热需求。NVIDIA的DGX B200和DGX H100 SuperPOD均标配液冷系统。每机柜功耗从H100时代的约40kW升至Blackwell时代的120kW+，迫使数据中心重构电力供给和冷却系统设计——直接液冷（Direct-to-Chip Liquid Cooling）和浸没式液冷（Immersion Cooling）正在成为新建AI数据中心的标准配置。

**架构层面的两个根本性变革方向**：

1. **稀疏计算（Sparsity）**：利用激活值和权重的稀疏性跳过无效计算。Volta引入的2:4结构化稀疏已使Tensor Core吞吐翻倍，未来非结构化稀疏的硬件支持将进一步提高效率。
2. **存算一体（Processing-in-Memory，PIM）**：将计算逻辑集成到DRAM或HBM堆叠中，减少数据在显存和计算单元间的搬运。三星HBM-PIM和AMD的3D V-Cache是这一方向的最前沿尝试。

## 核心瓶颈

**功耗与散热**是GPU技术演进最直接的物理约束。从GeForce 256（约20W）到B200（约1200W），单卡功耗增长了60倍。机柜级功耗从过去的10kW量级跃升至120kW+，一个万卡GPU集群的电费年支出可超过1亿美元。这要求数据中心重新选址（靠近水电/核电），并采用更高电压供电方案。

**存储器带宽**是另一个关键短板。HBM（High Bandwidth Memory）的容量和带宽增长速度跟不上GPU的计算吞吐增长。A100的HBM2e带宽为2.0TB/s，到H100升级为HBM3（3.35TB/s），但Tensor Core性能增长远超这一幅度（TF32下A100 156 TFLOPS vs H100 FP8 2000 TFLOPS，带宽增速仅为计算增速的1/5）。HBM4预计在2027年才能大规模量产，且产能受限于SK海力士和三星的良率爬坡。

**先进封装**产能（CoWoS，台积电）是当前AI GPU出货量的最大瓶颈。Blackwell需要CoWoS-L封装（桥接芯片嵌入硅中介层），工艺复杂，每片晶圆的产出芯片数远低于传统封装。2024年CoWoS产能已扩充至约40万片/年，但仍供不应求。NVIDIA下一代Rubin将需要更先进的封装形式，产能矛盾在短期难以缓解。

**地缘政治**因素为供应链增加了不可预测性。BIS（美国商务部工业安全局）于2022年10月和2023年10月两次升级对华出口管制，H100/A100/H800等高端AI GPU被列入出口限制清单[11]。NVIDIA为此推出降规版本A800/H800（NVLink带宽减半），2023年底连降规版本也被限制，进一步推出H20（对比H100算力缩减约80%）。这使得中国AI企业面临"买不到"和"买得起的算力不够"的双重困境，倒逼华为昇腾、寒武纪等国产AI加速器加速追赶，但制程工艺（7nm vs 4nm）和CUDA兼容性仍是难以跨越的鸿沟。

## 产业影响

GPU的技术演进并非孤立发生于半导体领域，其影响已扩散到整个科技产业格局：

**对AI产业的底层支撑**：GPT-3（1750亿参数，约1000 PFLOPS·天的训练算力）到GPT-4（预计数万亿参数，约10000+ PFLOPS·天的训练算力）的跃迁完全依赖于GPU代际升级带来的算力密度提升。每一代GPU（约2年周期）带来的训练吞吐翻倍，直接转化为大模型参数规模和训练速度的同步增长。GPU是AI能力的**通货**——谁的GPU多，谁的模型就大，谁的AI能力就越强。

**NVIDIA的市场地位**：NVIDIA在AI GPU的市场份额估计超过80%（2025年数据），毛利率高达75-78%，市值于2024年一度突破3万亿美元，超过英特尔、AMD、高通之和。其竞争优势已从单一的硬件架构领先扩展为**硬件+软件（CUDA/TensorRT）+网络（NVLink/Mellanox/InfiniBand）+系统（DGX/Base Command）**的垂直整合策略，竞争对手在任一环节都难以实现全面突破。

**A股产业链映射**：

国内A股市场没有纯正的GPU芯片设计标的（海光信息DCU属于GPGPU加速器，与NVIDIA的直接竞争关系弱，且制程落后约2代；景嘉微主打军用GPU，规模差距超过2个数量级）。产业链受益主要集中在以下环节：

- **GPU服务器整机**：工业富联（NVIDIA DGX代工核心供应商，营收占比约30%来自AI服务器）、浪潮信息（国内AI服务器市场份额第一，采用NVIDIA和昇腾GPU）
- **AI服务器散热**：英维克（液冷解决方案，已进入NVIDIA DGX供应链）、高澜股份（服务器液冷，与曙光系合作紧密）
- **光模块与网络互联**：中际旭创（800G光模块核心供应商，NVLink光互联趋势受益者）、天孚通信（FA/MT光引擎，光模块上游精密器件）
- **PCB与封装基板**：沪电股份（AI服务器高多层PCB）、兴森科技（FCBGA封装基板，Chiplet封装材料国产替代概念）
- **数据中心电力**：科华数据（UPS）、科士达（HVDC电源），受益于AI数据中心电力扩容

需要注意的是，上述公司的GPU业务占比和实际受益程度差异很大，部分公司涉及概念炒作成分多于实质业绩贡献。

## 参考文献

[1] NVIDIA Corporation. NVIDIA Launches the World's First Graphics Processing Unit: GeForce 256[R]. NVIDIA Press Release, August 31, 1999.

[2] KRUEGER J, WESTERMANN R. Linear Algebra Operators for GPU Implementation of Numerical Algorithms[J]. ACM Transactions on Graphics, 2003, 22(3): 908-916.

[3] NVIDIA Corporation. NVIDIA GeForce 8800: The First Unified Shader Architecture[R]. NVIDIA White Paper, November 8, 2006.

[4] NVIDIA Corporation. NVIDIA Tesla V100 GPU Architecture: The World's Most Advanced Data Center GPU[R]. NVIDIA White Paper, 2017.

[5] KRIZHEVSKY A, SUTSKEVER I, HINTON G E. ImageNet Classification with Deep Convolutional Neural Networks[C]//Proceedings of the 25th International Conference on Neural Information Processing Systems (NeurIPS). Lake Tahoe, 2012: 1097-1105.

[6] NVIDIA Corporation. NVIDIA A100 Tensor Core GPU Architecture[R]. NVIDIA White Paper, 2020.

[7] NVIDIA Corporation. NVIDIA H100 Tensor Core GPU Architecture[R]. NVIDIA White Paper, 2022.

[8] NVIDIA Corporation. NVIDIA Blackwell GPU Architecture[R]. NVIDIA White Paper, 2024.

[9] NVIDIA Corporation. FY2023 Annual Report (Form 10-K)[R]. U.S. Securities and Exchange Commission, February 22, 2023.

[10] HUANG J. NVIDIA Keynote at GTC 2024: Revealing Next-Generation GPU Architecture[R]. NVIDIA GTC, March 18, 2024.

[11] Bureau of Industry and Security, U.S. Department of Commerce. Export Controls on Advanced Computing and Semiconductor Manufacturing Items: Interim Final Rule[R]. Federal Register, October 7, 2022.

[12] SUTSKEVER I, VINYALS O, LE Q V. Sequence to Sequence Learning with Neural Networks[C]//Proceedings of the 27th International Conference on Neural Information Processing Systems (NeurIPS). Montreal, 2014: 3104-3112.

[13] JOUPPI N P, YOUNG C, PATIL N, et al. In-Datacenter Performance Analysis of a Tensor Processing Unit[C]//Proceedings of the 44th Annual International Symposium on Computer Architecture (ISCA). Toronto, 2017: 1-12.

[14] 徐磊. GPU计算: CUDA与OpenCL编程[M]. 北京: 机械工业出版社, 2012.
