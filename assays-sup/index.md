# Assays Sup


## PDB
### 隐藏侧链
### 比对结构
```
align ClassA_gpr75_human_Active_7BU7_2021-11-02_GPCRdb, 3sn6 and chain R（指定受体）
fetch 3sn6
align ClassA_gpr75_human_Active_7BU7_2021-11-02_GPCRdb, 7rmg and chain R（匹配的不好，NK1R helix8在膜上应该是水平而非翘起）

create gfp, 3WLD and resi 59-302 （分离两个部分，可以隐藏其中一个）
```

> β2AR-Gs (pink, PDB 3SN6)         
> peptide ligand–free state of GCGR (Fab-bound GCGR, blue, PDB 5XEZ)        
> partial agonist NNC1702-bound GCGR (orange, PDB 5YQZ)
> GCGR GDP-bound Gαs (green, PDB 6EG8)
> Comparison between the interaction of ICL2 of GCGR (cyan) and β2AR (pink) (PDB 3SN6)


#### 完全激活态 ZP3780 （GCGR-Gs）
特征：
1. H-bonds (dotted lines) with residues in TM1, TM3, TM7, and ECL3 (cyan)
2. The presence of H1 in ZP3780 ensures interaction with Q232 and may induce rearrangement of residues in **TM5**. 
3. interaction of D9 seems to stabilize **TM7 and ECL3** displacement, which might trigger GCGR activation. 
4. R378对激活起决定性作用
5. Substantial structural change is observed in **TM6**, which moves outward by 18 Å and partially unwinds to form a kink with an angle of 105°.

#### 部分激活态 partial-agonist NNC1702 （5YQZ）
特征：
1. 和完全激活比， 没有H1 and has a D9E mutation
2. 没有TM6 kink的向外摆（Reorientation of residues in TM3, TM5, and TM7 around the PxxG motif加速TM6变化）T351来度量

#### 非激活态 Fab-bound GCGR（5XEZ）
1. **PxxG motif** (P3566.47-L3576.48-L3586.49-G3596.50) 

#### b2R激活的Gs结构变化
1. ClassB：GDP-bound Gαs / nucleotide-free Gαs
> 	后者**α5 helix** to engage the receptor core, and conformational rearrangements in the α1 helix and α5-β6 loop. 产生extensive polar interactions with TM5, TM6, and helix 8 (H8)。
2. 在ClassB中是`丙氨酸`，hydrophobic pocket less efficiently；在ClassA中是`苯丙氨酸`F139 on ICL2，参与hydrophobic pocket lined by residues from the **αN-β1 loop, the β2-β3 loop, and the α5 helix**

## Assay

### GDP解离实验

> GPCR-Gs-GDP复合物发生核苷酸水解，GDP才disassociate.        
> A/B GPCR都在HDL particles和在去垢剂中的G蛋白亚基中进行重构。

- Gas：Tcep/GDP，加大量的3H-GDP到终浓度2.5uM，孵育一小时         
- 再加Gbr：DDM/Tcep，孵育30min        
- GPCR in HDL：2mM GTP，100uM agonist，1h孵育   
- GDP load的G蛋白三聚体，5uM GPCR混合
- 不同的时间点，500ul wash buffer add to 20ul reaction
- **microanalysis filter holder (EMD Milipore) and pre-wet mixed cellulose filters（滤膜）**        
- buffer洗，RT 1h晾干，残留的GDP放射性会bound to filters.         
- **liquid scintillation spectrometry(检测)**          

随着时间增加，解离下来的越多，膜上的荧光信号越强，代表bound的GDP越少。样品在闪烁液中引起闪烁，把核辐射能转换成光子。 


### Bodipy-GTPγS binding （GTP结合实验）
Bodipy-GTPγS是一个不水解的荧光标记的GTP类似物。在溶液中自己淬灭。结合G蛋白荧光会增强因为荧光团不淬灭。所以能看**nucleotide-free GPCR–G protein complexes**的GTP结合率。

对照：150 nM BODIPY-FL-GTPγS in the absence of receptor-G protein complexes for 100 s to establish the **baseline fluorescence intensity**    

实验：Nucleotide-free GCGR-Gs and β2AR-Gs complexes（**GDP的结合状态** 此时GTP荧光弱）      
- fluorescence form BODIPY-FL-GTPyS (Thermo Fisher) was recorded in 500 uL quarz cuvettes using the Fluorolog spectrophotometer (HORIBA). The fluorophore was exited at 495 nm and emission was detected at 508 nm at 22°C.      
- Receptor-G protein complexes were added to 1 μM快速混合，取出看kinetics spectra，理论上结合了G蛋白的GTP荧光信号会增强。          

随着时间增加，结合的GTP越多，和G蛋白结合的GTP被保护起来不会淬灭，荧光信号越强。

![](https://els-jbs-prod-cdn.jbs.elsevierhealth.com/cms/attachment/8176638d-2787-41fe-9519-a6722aef5b3d/gr1_lrg.jpg)


### DEER
用bis-(2,2,5,5-tetramethyl-3- imidazoline-1-oxyl-4-yl)disulfide (IDSL)标记受体；制样加ligand用80%的甘油冷冻起来，转移到硼硅酸盐毛细管。The observe pulses −70 MHz offset from the pump pulse，an echo nutation回声章动可以测定Optimal pulse lengths. L-curve criterion用于正则参数的距离分布确定。

测量DEER distance distributions；加了全激动剂之后也没有改变距离，说明TM5、6没有经历配体诱导的构像变化。

对比在b2中，DEER distance measurements between TM4 and TM6 have shown that agonist binding increases the population of the active state by **stabilizing the outward conformation of TM6**.

说明`higher energy level` of the fully active conformational state of GCGR in the presence of agonist alone compared with the β2AR 


## Bimane
bimane 荧光光谱的最大发射波长 (λmax)说明一些构象的改变。         
10uM Receptor + **5mol bimane or NBD**  
受体是最小的半胱氨酸 GCGR 构建体 (mC-GCGR)，以通过基于硫醇的化学进行位点特异性标记。          
size exclusion chromatography purify receptors to remove **label**。  
加了Gs后用cuvette检测受体Bimane，NBD荧光          
λmax change of ligand and Gs binding是独立的测量荧光受体的不同状态得到的。拟合的 λmax 值作为高斯拟合的平均值。       
**TM6向外摆荧光会下降。**

加了GDP/GTP后，尽管FRET 的变化导致 GCGR-Gs 复合物几乎完全解离，但是TM6没有复位，Bimane的信号还是很低。




