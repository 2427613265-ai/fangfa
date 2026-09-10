# 论文实质性错误质检报告

- **稿件**：Spatiotemporal Patch Transformer with Differentiable Recursive Feature Selection for NDVI Cloud-Gap Filling（Geocarto International 投稿稿，Submission ID 269427137）
- **质检范围**：科学论断、物理机制、实验设计与数字自洽、方法学声称、图表—正文一致性、关键引用。不把英语润色、排版美观当作“实质性错误”，但公式崩溃若导致公式本身写错，仍计入。
- **结论先行**：**存在多处实质性错误**。其中至少 4 类会直接动摇摘要/贡献点（SAR–光学互补、空间邻域增益、不确定性数值、Conformal 保证）。按当前稿件，不宜视为可发表终稿。

---

## 1. 致命级：图表与正文互相否定

### 1.1 Figure 2 的 Pearson 系数推翻“SAR 与 NDVI 互补”的核心物理叙事

正文 2.4 / 摘要声称：VV、VH 后向散射随 NDVI 升高而**下降**，二者“方向相反、信息互补”，并作为 SAR 填云的物理依据。

**同一页 Figure 2 图内标注**：

| 极化 | 图内 Pearson r |
|---|---|
| VV vs NDVI | **−0.014** |
| VH vs NDVI | **+0.020** |

n = 50,000 时，|r|≈0.01–0.02 的效应量约为 0，R² < 0.0005。这不是“弱相关”，而是**没有可用的线性互补关系**。

因此下列句子在实证上不成立：

- “backscatter in both VV and VH polarizations tends to decrease as NDVI increases”
- “this provides direct empirical evidence for using SAR to assist optical cloud gap filling”
- 4.2 / 结论中的 “highly consistent with the Pearson correlation between individual modalities and NDVI (**r = 0.75**)”

Figure 2 已经给出 VV/VH 与 NDVI 的 r≈0，文中再写 r = 0.75，属于**数据与结论双重错误**（要么看错图，要么把未给出的“门控权重 vs 某组相关系数”误写成“各模态与 NDVI 的相关系数”）。

**影响**：摘要里 “learned gate weights aligning closely with physical sensor significance” 失去数据支撑。

### 1.2 Figure 12 与 4.4 节不确定性数值差一个数量级

Figure 12 左轴为 *Calibrated uncertainty (std)*，刻度约 **0.005–0.025**，柱高大致在 0.01–0.02。

正文却写：

> spring green-up … approximately **0.20** … mid-summer … **0.29–0.32**

这些数在该图上根本画不下。讨论 5.3 又写 cloud-mask ratio 下 std 为 **0.231–0.244**，同样与 Figure 12、Table 6 对不上。

用 Table 6 反推：95% MPIW = 0.100，区间半宽为 \(s\cdot 1.96\cdot\sigma\)，\(s=3.835\)，则平均 \(\sigma\approx 0.0067\)。与正文 0.20–0.32 差约 **30 倍**，与 Figure 12 的 ~0.015 也差约 2 倍。

**判定**：物候—不确定性的定量结论目前不可信；0.20/0.29 极像把 0.020/0.029 看错，或把 NDVI 曲线读成了不确定性。

### 1.3 Figure 3 不是正文所描述的“典型单峰物候”

正文：4 月 NDVI≈0.35，6–7 月升至 ≈0.68，8 月后降至 ≈0.61，并称 Figure 3 为 *smoothed phenological profiles*。

Figure 3 实际是各区 **cloud-free mean 的剧烈锯齿**（约 0.1–0.8 间反复跳变），看不出稳定单峰，也看不出平滑。高云量时段（Figure 4 可达 >50% 甚至近 100%）只剩少量晴空像元，区域均值会剧烈跳动——这能解释图形，但**不能**再把该图称作平滑单峰物候基线。

### 1.4 图号引用错误（会误导审稿人核对证据）

| 正文 | 实际图 |
|---|---|
| 4.1 “Training convergence curves (**Figure 7**)” | Figure 6 才是训练曲线；Figure 7 是 MAE 分布 |
| 2.4 后向散射随 NDVI 下降 “(**Figure 3**)” | 应为 Figure 2；Figure 3 是 NDVI 物候 |
| Figure 6 标题 “**four** models” | 图中只有 LSTM / Transformer / iTransformer 三条曲线，**缺 CNN-RNN** |

---

## 2. 致命级：方法学声称与真实算法不符

### 2.1 所谓 Split Conformal Prediction 并不是 CP

3.4 节写法是：

\[
[\hat y_i - s\cdot z_{1-\alpha/2}\hat\sigma_i,\; \hat y_i + s\cdot z_{1-\alpha/2}\hat\sigma_i]
\]

并“在校准集上搜索 \(s\) 使覆盖率接近名义水平”，再声称：

> distribution-free … finite-sample coverage guarantees under the exchangeability assumption.

问题：

1. **标准 split CP 不使用正态分位数 \(z_{1-\alpha/2}\)**（1.96 等）。那是高斯区间。
2. 真 CP 的分位数来自校准残差（或 \(|y-\hat y|/\sigma\)）的经验分位，不是“搜一个 \(s\) 去贴 PICP”。
3. 若只是方差缩放，**同一个 \(s\) 应适用于所有置信水平**。Table 6 中 \(s\) 随 80/90/95/99% 变成 2.854 / 3.265 / 3.835 / 5.569，说明只是**按置信水平分别拟合覆盖率**，不是单一共形校准。
4. NDVI 时间序列与地块在时空上强相关，**exchangeability 不成立**，即便真用 CP 也不能直接主张有限样本保证。

**判定**：把高斯区间校准误称为 Split Conformal Prediction，并调用了不适用的理论保证。这是方法学实质性错误，不是措辞问题。

### 2.2 “Differentiable RFE” 既不 recursive，也不 elimination

实现是通道级 sigmoid 软门控（式 1–2），\(\tau=1\)，推理时**不硬选择、不丢通道**。这是 channel gating / 可学习缩放，不是 Guyon 等（2002）的 RFE，也不是 Gumbel-Softmax / Concrete 离散选择（参考文献里引了 Jang、Maddison，正文未用）。

门控值 NDVI 0.380、RATIO 0.333、CROSS 0.298、VH 0.267、VV 0.223，全部挤在 0.22–0.38，**没有稀疏、没有淘汰**。NDVI 初始化 \(\theta=2\Rightarrow g\approx0.88\)，训练后降到 0.380，与“光学主导”的叙事也不吻合（最多是“略高一点”）。

### 2.3 Table 8 五年模态权重在微调后小数点后三位全部 “Unchanged”

对 2021 做 fine-tune 后 5 个门控与 2020 **完全相同到 0.001**。除非门控被冻结（文中未说），否则几乎不可能。这更像是复制 2020 数字，或未真正更新门控却据此声称：

> the “optical-dominated, SAR-assisted” fusion mechanism remains stable across interannual transitions  
> parameters calibrated once can be directly reused in subsequent years

把该表作为“年际物理机制稳定、一次标定多年复用”的证据，当前不可接受。

---

## 3. 严重级：实验逻辑不支持核心贡献

### 3.1 “空间邻域带来额外增益”与自己的表矛盾

| 设置 | MAE | R² |
|---|---|---|
| 像素级 LSTM（Table 3） | **0.0111** | 0.9622 |
| 像素级 Transformer | **0.0135** | 0.9525 |
| 提出的 Patch + Diff.RFE + MC（Table 4 B4） | **0.0166** | 0.9821 |

正文 3.3、5.2、结论反复说：像素级时间信息已饱和，引入 5×5 邻域可提供 additional gains / superior performance。

在未做 **同一骨干、同一评估点** 的 pixel vs patch 对照时，现有数字显示 patch 模型 **MAE 更差**。R² 更高可能只是评估子集/方差不同（像素级 365,298 点 vs patch 171,506 点），不能用来宣称空间建模胜利。

这是贡献点 1（Patch 时空 Transformer）的直接威胁。

### 3.2 与 Tsardanidis 的“公平同数据对比”不成立

稿件强调采用相同地块边界、**一致的 Sentinel-1/2 输入** 和统一评估协议。对照原文（Tsardanidis et al., *Comput. Electron. Agric.* 230:109732）：

| 项目 | Tsardanidis | 本稿 |
|---|---|---|
| 特征 | \(\sigma^0\)、**相干性**、RVI、ratio、cross-ratio、mixed coherence | 仅 VV/VH/RATIO/CROSS/NDVI，**无相干性** |
| cross-ratio | \(\sigma^0_{VH}-\sigma^0_{VV}\) | CROSS = **VV−VH**（符号相反） |
| 预处理 | snappy + Refined Lee + SRTM；SLC 相干 | GEE `S1_GRD` / `S2_SR_HARMONIZED` |
| 验证样本数 | **365,298**（1,022 地块） | 像素级实验也写 **365,298** |

像素级验证集样本量与 Tsardanidis **完全相同**，训练集约 824,978 vs 原文 983,302；方法节又写全数据 8:2（对应约 1,067 验证地块，Table 5）。更像像素级沿用了 Tsardanidis 的划分，patch 实验用了另一套 8:2，却写成 “unified data partitioning”。

此外，他们承认 GEE 没有相干产品（5.6），却在 2.3 写输入一致。基线 CNN-RNN MAE=0.0238 与原文 0.024 接近，只能说明量级可复现，**不能**把增益完全归因于邻域建模和可微特征选择。

### 3.3 像素级样本协议自相矛盾

方法：8:2、seed=42，增强后训练样本 42,710（地块×10 mask，与 4,271×10 相符）。

4.1 像素级：824,978 train / **365,298** val，比例约 **69:31**，不是 8:2。365,298 与 Tsardanidis 验证集锁死，更像未重新抽样。

---

## 4. 严重级：遥感物理与特征定义

### 4.1 散射机制写反

> volume scattering and depolarization … increase, **leading to a general decrease in backscatter**

体散射增强通常使 \(\sigma^0\)（尤其 VH）**升高**；若观测到下降，主流解释是**土壤散射被冠层衰减**，并常与土壤湿度、物候混杂。把“体散射↑ → 后向散射↓”写成一般物理规律是错的。结合 Figure 2 r≈0，该节整体不成立。

### 4.2 dB 域做 RATIO = VV/VH 没有极化物理意义

Table 1 与 2.2：\(\sigma^0\) 为 **dB**，同时又定义

- CROSS = \(\sigma^0_{VV}-\sigma^0_{VH}\)（这才是线性比的 dB 形式，可用）
- RATIO = \(\sigma^0_{VV}/\sigma^0_{VH}\)（两个 dB 相除，**不是**标准 \(\sigma_{VV}/\sigma_{VH}\)）

VV、VH、CROSS、RATIO 由同一对观测**函数依赖**（有效自由度是 NDVI+VV+VH）。讨论 5.1 承认共线性，但 4.2 仍把五门控排序解释为独立物理贡献，并让非物理的 RATIO（0.333）排第二。这会让“可解释性”结论站不住。

### 4.3 合成策略与“无缺口”自相矛盾

2.2：SCL 仅保留 4/5，再对窗口取中值，以 “**guarantee valid values for every time step**”。

Table 2 / Figure 4：合成后仍有 26.0%（2020）和 34.5%（2021）云缺口，部分时相 >50%。中值不能把全云窗口变成有效值。该句为假。

立陶宛约 54–56°N，属**中纬度波罗的海**，不是 Nordic，也不是通常意义上的 high latitude（>60°N）。“northern high-latitudes / nordic high latitudes / high-latitude zone of Northern Europe” 作为问题设定不准确（云多可以保留，地理标签应改）。

气候区：2.1 写 **four** major climatic zones，2.3 又写 parcels 覆盖 **six climatic regions**。六块是研究区，不是六种气候型。

---

## 5. 中等级：数字、引用与可重复性

### 5.1 结论与正文百分比不一致（次要，但属于结果表述错误）

| 指标 | 4.2 节 | 结论第 2 点 |
|---|---|---|
| MAE 降幅 B1→B3 | 22% | **23%** |
| RMSE 降幅 | 20% | **19%** |

按 Table 4：(0.0217−0.0169)/0.0217 = 22.1%，(0.0358−0.0288)/0.0358 = 19.6%。应以表为准，结论写错。

Table 3 的 43%、53% 误差下降计算正确。Table 2 分区云量/NDVI 加权平均与 “All” 行一致（26.0%、0.516），这一块是对的。

### 5.2 引用不实或张冠李戴

- **Košánová et al., 2025**（*Geocarto International* 40:2532529）被用来支持 “Sentinel-1/2 fusion … soil-moisture retrieval and yield-stability”。该文是斯洛伐克 **Sentinel-2 NDVI 产量稳定性**，**没有 S1、没有土壤水分反演**。
- **El Youssfi et al., 2026**：实际第一作者为 **Benzhair**；Elyoussfi 为第二作者。且是作物制图综述，不是 gap-filling。
- **Kaplan & Avdan, 2018**：刊物写成 “Remote Sensing, The International Archives…”，实为 *Int. Arch. Photogramm. Remote Sens. Spatial Inf. Sci.* XLII-3，题目是湿地**制图**而非 monitoring。
- 正文 Tsardanidis **(2024)**，参考文献为 **2025**（期刊 2025 年 3 月卷，在线 2024-12）。可统一，但需与 DOI 一致。
- 结论公式 “features and NDVI (** = 0.75**)”，相关系数符号丢失。

### 5.3 评估设计不能支撑“业务化填云”

- 只在**晴空位置上再随机挖 30%** 算 MAE，不是真实成片、持续数周的云斑。
- 引言以割草事件检测为动机，**没有做任何下游 mowing 实验**（Tsardanidis 的核心评估之一）。
- 无多种子、无置信区间；B3/B4 的 0.0169 vs 0.0166 完全可能是训练噪声。
- Data and Code Availability 有标题无代码；声明 “no generative AI” 与公式被挤进正文末尾的残骸（见下）高度不协调，审稿人可能追问。

### 5.4 公式排版已损坏（提交质量，部分会改变公式含义）

第 10–12 页公式后残留 `X∈R^{B×T×H×W×C}C=5cθ_c`、`g∈R^C c=5 θ_5=2.0 g_c`、`p=0.1 N=50` 等碎片，是 MathType/LaTeX 转换失败。式 (4)(10) 在图面中**有**平方根，文本抽取会误判；但式 (5) 区间两个端点挤在一起、式 (11) 分母不清晰，审稿人很难核验。这不一定是数学推错，但**当前 PDF 不能作为可审公式版本**。

---

## 6. 内部一致、可以保留的部分

- 29×5×5=725 tokens；B1/B2/B3 参数差 128 / 5，与“少两通道 / 五个 logit”相符。
- Table 2 各区 Parcel×25 = Patch pixels；云量、NDVI 加权平均正确。
- Table 5 验证地块合计 1,067；Telšiai 云量最高、每地块评估点最少，方向合理。
- Table 4 的 val_loss 与 RMSE：\(\sqrt{0.00085}\approx0.0291\)，\(\sqrt{0.00128}=0.0358\)，计算自洽。
- 相对 CNN-RNN 的 43%/53% 降幅计算正确。
- 立陶宛面积 ~65,300 km²、C 波段波长 ~5.6 cm、GEE 产品名、Zenodo DOI `10.5281/zenodo.11651601`、Tsardanidis MAE≈0.024 / R²≈0.92 与原文一致。
- iTransformer 把时间压成变量 token，对**填补**任务不合适——方向性解释合理，但 “systematically fails” 缺少调参对照，只能当弱证据。

---

## 7. 对投稿决策的含义

| 贡献点（稿件自我定位） | 质检结果 |
|---|---|
| SAR–光学物理互补 + 门控可解释 | **不成立**（Figure 2 r≈0；r=0.75 无来源；RATIO 定义可疑；Table 8 不可信） |
| Patch 时空建模优于像素序列 | **未证明，现有表还偏向反例** |
| 可微 RFE | **名实不符**（软门控，无递归淘汰） |
| MC Dropout + Split CP，PICP=0.952 | 覆盖率数字可以是“调 s 贴出来的”；**理论声称错误**；物候不确定性数字与图冲突 |
| 跨年 zero-shot MAE +0.6% | 数字内部自洽，但与错误的门控稳定性叙事绑在一起，需独立复核 |

**建议（若仍要投）必须先改，否则大修/拒稿风险很高：**

1. 按 Figure 2 重写 2.4：承认线性相关≈0，改用偏相关、分层（湿度/物候/入射角）或干脆降级互补叙事。删除 r=0.75 或给出可复核的五元相关表。
2. 增加 **pixel vs 5×5 patch、同一 Transformer、同一 mask 点** 的对照；若 patch 不更好，就不要把邻域当主要贡献。
3. 把方法改名为 learnable modality gates；不要称 RFE/CP，除非改成真正的共形分位数（去掉 \(z_{1-\alpha/2}\)，用校准残差分位）。
4. 重画/重读 Figure 12，统一 \(\sigma\)、MPIW、s 的单位；修正 0.20/0.29 这类数量级错误。
5. 披露与 Tsardanidis 的特征/预处理/数据划分差异；CROSS 符号、RATIO 的线性/dB 域写死。
6. 重排全部公式；修正图号、气候区数量、Košánová/Benzhair 引用。
7. Table 8 给出真实微调后的门控，或写明冻结。

---

## 8. 质检方法说明

- 通读投稿 PDF 26 页（含封面），并对公式页、Figure 2/3/6/9/12、Table 2–8 做了渲染核对。
- 对照 Tsardanidis et al. 2025 原文与 Zenodo 10.5281/zenodo.11651601、Košánová 2025、Benzhair & Elyoussfi 2026、Kaplan & Avdan 2018 的书目与摘要。
- 抽查了加权平均、MAE 降幅、MPIW 与 \(s\cdot z\cdot\sigma\) 的量纲关系。

未复现训练代码（稿件未提供），因此 Table 3–7 的**原始计算**无法独立重跑；上述错误均来自稿件内部矛盾或与公开文献冲突，不依赖重训。
