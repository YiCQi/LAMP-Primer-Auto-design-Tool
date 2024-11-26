# LAMP-Primer-Auto-design-Tool

### 1. **理解LAMP引物的设计需求**：
![Pasted image 20241106154754](https://github.com/user-attachments/assets/53370d77-0a43-437c-82fa-0a87d77aef73)
- **引物数量与类型**：LAMP扩增需要六个引物：前内引物 (FIP)、后内引物 (BIP)、两个外引物 (F3和B3)，以及两个环引物 (LF和LB)。这些引物是用来锁定特定的靶序列区域的。
- **引物的间距和方向**：LAMP引物设计要求非常严格的间距和结合模式，比如FIP和BIP要在目标序列上相隔一定的距离，确保高效的环状结构形成，从而实现等温扩增。

### 2. **主要挑战**：

- **复杂的设计限制**：LAMP引物设计比传统的PCR引物复杂，需要考虑多个因素：
    - **特异性**：引物只能结合在目标序列上，不能在其他类似序列上扩增。
    - **熔解温度 (Tm)**：最佳范围是58-63°C。
    - **GC含量**：理想值是40-60%。
    - **ΔG（自由能）**：需要保持低ΔG，防止引物二聚体和二级结构的形成。
- **多菌株兼容性**：设计出的引物要能在不同菌株中都有效，这样才能在多样性样本分析中使用。

### 3. **算法设计思路**：

- **定义限制条件和评分标准**：
    - 对每个候选引物进行约束条件的评估（如Tm、GC含量），并根据其符合程度赋予分数。
    - 可选加入二级结构预测和二聚体形成过滤，这将进一步提高评分准确性，剔除潜在问题引物。
- **输入数据**：
    - 工具输入包括靶序列的比对信息（你希望扩增的区域）和背景基因组信息（你不希望扩增的区域）。
- **输出结果**：
    - 工具将输出排好序的引物组合，按分数从高到低列出最佳候选供用户选择。

### 4. **潜在的算法和工具**：

- **动态规划**：可以用来对候选引物和靶序列及背景序列进行比对，确保特异性。
- **热力学计算**：使用最近邻居模型等方法来计算ΔG和Tm。
- **机器学习（可选）**：如果收集了足够的LAMP有效引物数据，可以训练模型来预测最佳引物组合。

### 5. **基准测试和验证**：

- **文献验证**：参考已发表的LAMP引物，验证工具能否对它们给出较高评分。
- **模拟和实际测试**：可进行计算机模拟扩增，条件允许的情况下还可进行实验室验证。

## 目前思路：

### 算法可行性：

- **TM**: 
Tm is estimated using the Nearest-Neighbor method. This method is currently considered to be the
 approximation method that gives the value closest to the actual value. 
The calculated Tm is affected by experimental conditions such as the salt concentration and oligo concentration,
 so it is preferred that Tm be calculated under fixed experimental conditions (oligo concentration at 0.1 µM, sodium
 ion concentration at 50 mM, magnesium ion concentration at 4 mM). 
The Tm for each region is designed to be about 65°C (64 - 66°C) for F1c and B1c, about 60°C (59 - 61°C) for F2,
 B2, F3, and B3, and about 65°C (64 - 66°C) for the loop primers. 
- **GC**%:  --> 40-60
- **ΔG**:  --> ![Uploading image.png…]() <-4
- **二级结构**: 使用现有的工具，例如RNAfold或mfold, 来预测自由能变化，以便找出引物的可能二级结构（例如：发夹结构、二聚体等）。
- ...

- **评分系统**: log损失函数/机器学习

### Benchmark 三步：

1. primer可行性：取任何published文章中使用的primer，投入我们的工具，检查primer的分数是否较好；
2. 工具可行性：取published文章中的扩增序列与选用的primer，将扩增序列投入检查是否结果中有对应primer且分数良好；
3. 各工具对比：取相同扩增序列若干，投入PrimerExplorer5, NEB LAMP and PremierBiosoft，检查各个工具结果是否类似。
