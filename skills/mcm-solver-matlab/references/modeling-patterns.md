# 建模模式速查

以下面向常见数学建模题型给出选型信号、MATLAB 原生函数/工具箱、默认要素和典型陷阱，主技能在决策时可以直接引用此处内容。

## 1. 优化
- 适用场景：资源分配、调度、网络流、运筹等需要极小化成本或极大化收益的问题；目标函数、约束明确、可用解析表达式。
- MATLAB 函数/工具箱：`fmincon`、`ga`（Global Optimization Toolbox）、`patternsearch`、`linprog` / `quadprog`、`optimproblem` + `solve`
- 默认输出要素：目标函数值、最优变量向量、KKT 状态、灵敏度（`output.maxconstraint`、`lambda`）、收敛历史图。
- 常见坑：未规范化量纲导致优化路径不稳定；边界只能用 `lb/ub`，需明确数据类型；若梯度缺失，优先用 `ga` 或 `patternsearch` 作为备选。

## 2. 预测 / 拟合
- 适用场景：时间序列预测、回归建模、机器学习回归任务，需要拟合输入与输出之间的关系。
- MATLAB 函数/工具箱：`fitlm`、`lasso`、`ridge`、`polyfit`、`nlinfit`、`regstats`、`System Identification Toolbox` 中的 `iddata` + `ssest`。
- 默认输出要素：模型系数、`R^2` / `RMSE`、残差分布、预测区间、数据拆分方式（训练/验证/测试）、`plotResiduals`。
- 常见坑：未处理多重共线性、变量未标准化；忽略异方差或自相关；未提供可复现的随机种子（`rng`）。

## 3. 统计分析
- 适用场景：假设检验、分组对比、蒙特卡洛稳健性、敏感度分析。
- MATLAB 函数/工具箱：`ttest`, `anova1`, `chi2gof`, `bootstrp`, `random`, `histfit`, `statistics Toolbox` 中的 `fitdist`。
- 默认输出要素：检验统计量、p 值、置信区间、可视化（箱线图、直方图、概率纸）、重抽样结果。
- 常见坑：忽略样本量小的偏差；多次比较时未校正；使用 `rand` 时未设置 `rng` 使结果不可重复。

## 4. 微分方程
- 适用场景：动力系统、传播模型、热传导、生态模型；需要求解初始/边值问题的连续变化。
- MATLAB 函数/工具箱：`ode45`, `ode15s`, `ode23s`, `pdepe`, `bvp4c`、Simulink 程序接口，`symbolic` 中的 `dsolve` 用于验证解析形式。
- 默认输出要素：时间步列表、状态变量曲线、稳定性指标、步长控制、当 `ode15s` 发生刚性时的备选方案。
- 常见坑：未指定相对/绝对误差（`odeset`）、初值不合理导致发散、刚性问题没有换用 `ode15s`。

## 5. 图论 / 网络
- 适用场景：路径优化、网络流、社交网络影响力分析、城市模型。
- MATLAB 函数/工具箱：Graph 类 (`digraph`, `graph`)、`shortestpath`, `maxflow`, `centrality`, `conncomp`、`Bioinformatics Toolbox` 的 `graphaligner` 也可引用。
- 默认输出要素：图结构（节点/边）、度分布、关键路径、可视化（`plot` + `layout`）、网络健壮性分析指标。
- 常见坑：边权重与方向处理不一致；未检查图是否连通；大型图需控制绘图粒度避免卡死。

## 6. 评价模型
- 适用场景：指标体系构建（熵权、层次分析、TOPSIS）、绩效评分、风险排序。
- MATLAB 函数/工具箱：矩阵归一化（`normalize`）、特征值分解（`eig`）、`rank` / `svd`、数据降维（`pca`）、`fuzzy` 工具箱的 AHP 分析。
- 默认输出要素：权重矩阵、排序结果、稳定性敏感度、说明性表格和雷达图展示。
- 常见坑：权重在构建时没有归一化；指标之间存在多重共线性；未说明标准化方法（极差、Z-score）。

## 7. 仿真
- 适用场景：离散事件仿真、车间调度仿真、人群流动、系统动力学；强调时序演化和随机性。
- MATLAB 函数/工具箱：`simulink`, `sim`（通过 `mdl` 模型）、`randstream`, `event-based` 逻辑；纯代码下使用 `for` 循环 + `pause` + `animatedline`。
- 默认输出要素：迭代步骤、随机种子、指标随时间变化、每次仿真状态的快照、可视化（`animatedline`, `heatmap`）。
- 常见坑：忽略仿真步长对结果的影响；输出大量中间数据导致内存飙升；未提供结果存档（`save` / `matfile`）。
