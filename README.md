# CCL2022 汽车工业故障模式关系抽取评测

## 任务申请书

- 组织单位：
  - 达而观信息科技（上海）有限公司
- 任务组织者：
  - 陈运文（ 达观数据 ）
  - 文辉（ 达观数据 ）
  - 王文广（ 达观数据 ）
  - 王昊奋（同济大学）
- 任务联系人
  - 王小荻：wangxiaodi@datagrand.com

#### 如何报名

[**点击此处填写信息报名**](https://docs.qq.com/form/page/DWFNTcWdGbVRrZWlZ)


### 1. 任务内容

实体抽取和关系抽取是信息抽取的基础任务，面向汽车故障领域的信息抽取对于实现智能化检修和诊断具有重大意义。汽车故障领域案例文本是由维修从业人员撰写的描述汽车功能异常、排查步骤的记录，该记录包括故障现象、故障原因以及排故过程等，故障案例知识的重复利用受到数据结构化程度的影响，因而识别数据中的部件单元、性能表征、故障状态等核心实体及其组合的故障模式关系至关重要。

通过从大量故障案例文本抽取出部件单元、性能表征、故障状态等实体及其故障模式，可以为后续故障知识图谱构建和故障智能检修和实时诊断打下坚实基础。本任务需要从故障案例文本自动抽取2种类型的关系和3种类型的实体。关系类型为：部件单元的故障状态、性能表征的故障状态。故障模式关系的具体定义如下：

实体类型：
| 实体类型名称 | 说明                                   | 示例                           |
| :----------- | :------------------------------------- | :----------------------------- |
| 部件单元     | 包括汽车的各系统部件、单元、零件等     | “燃油泵”、“火花塞”、“发动机”   |
| 性能表征     | 部件的特征或者性能描述                 | “压力”、“转速”、“温度”         |
| 故障状态     | 系统或部件的故障状态描述，多为故障类型 | “漏油”、“熄火”、“变形”、“卡滞” |

故障模式关系类型：

<table>
<thead>
    <tr>
        <td rowspan=2>主体</td>
        <td rowspan=2>客体</td>
        <td rowspan=2>关系</td>
        <td colspan=2>示例</td>
    </tr>
    <tr>
    <td>主体</td>
    <td>客体</td>
    </tr>		
</thead>
<tbody>
    <tr>
        <td>部件单元</td>
        <td>故障状态</td>
        <td>部件故障</td>
        <td>发动机盖</td>
        <td>抖动</td>
    </tr>
    <tr>
        <td>性能表征</td>
        <td>故障状态</td>
        <td>性能故障</td>
        <td>液面</td>
        <td>变低</td>		
    </tr>
</tbody>
</table>

### 2. 评测数据

本次评测任务的数据采用人工标注和专家复核的方式，确保语料标注样本质量。本任务提供的训练数据集和评测数据集均为文本文件格式，每行为一个故障关系（Json格式，text：故障文本内容，h：头实体相关信息，t：尾实体相关信息，name：实体名，id，实体的uuid，pos：实体在文本中的位置，relation：头实体和尾实体的关系）。评测数据集不包含relation字段。示例如下：

故障案例样本1：

```json
{
	"text": "拔掉车灯开关，大灯继续点亮，不能熄灭，说明不是灯光开关的原因", 
    "h": {
        "name": "大灯",
        "id": "68b65826-ad96-4996-9876-fe1d9c2a990e",
        "pos": [7, 9]
    }, 
    "t": {
        "name": "不能熄灭", 
        "id": "dca62d0e-1398-4a0a-9e87-7366be4bdc97", 
        "pos": [14, 18]
    },  	
    "relation": "部件故障"
}
```

故障案例样例2：
```json
{
	"text": "故障现象丰田吉普3400，加速不良分析诊断经检查怠还平稳，无机头事故灯，汽油压力在标准数据范围，易起动，缸压正常，如此初步诊断为油路过脏引起加速不良", 
    "h": {
        "name": "加速", 
        "id": "3ccb7de6-2b8e-40b3-8103-b846f80c2036", 
        "pos": [23, 25]
    }, 	
    "t": {
        "name": "不良", 
        "id": "27209fc7-63f7-4c9f-a9fd-5f2439b5cf8d", 
        "pos": [25, 27]
    }, 	
    "relation": "性能故障"
}
```

本任务提供的训练数据包括约800份的故障案例，每份案例包括一条或者多条故障模式关系，对于评测模型效果的评测方式为线下评测。参赛者在提交截止期前需要提供代码，程序输入为评测测试集文本，输出包含spo_list的结果文本。评测测试集包括超过200份的故障案例，验证指标为关系抽取的F1指标。

训练集一共包括3000条关系，其中两种故障模式关系的分布情况：

| 关系类型 | 数量 |
| :------- | :--- |
| 部件故障 | 2812   |
| 性能故障 | 188  |

### 3. 评价标准

请给出评测任务的评价标准（每个任务的评价标准，最终评测成绩的评价标准等）

本次评测任务采用精确率（Precision, P）、召回率（Recall, R）、F1 值（F1-measure, F1）来评估抽取效果。
对于每一种关系，相关的定义如下：

$$
识别关系的精确率 = \frac{识别关系与标注相同的数量}{识别关系总数量}
$$

$$
识别关系的召回率 = \frac{识别关系与标注相同的数量}{标注关系总数量}
$$

$$
关系抽取的F1 = 2 * \frac{识别关系的精确率* 识别关系的召回率}{识别关系的精确率+ 识别关系的召回率}
$$

识别关系与标注相同指两个三元组的subject、object和relation都相同，即主体、客体、关系类型都需要识别正确。

最终结果F1定义为各个实体的F1的宏平均：

$$
F1 = \frac{部件故障关系F1 + 性能故障关系F1}{2}
$$


### 4. 评测赛程

请给出评测的大致赛程（报名、提交、公布结果等时间节点）
预计5月31号之前准备好数据。其他大致时间点如下：
1.	报名截止时间：2022年7月31号
2.	提交截止时间：2022年8月31号
3.	公布结果时间：2022年9月30号

### 5. 评测奖励

本测评总奖金**2万元**：
- 一等奖 （一名） 8000
- 二等奖 （两名） 4500
- 三等奖 （三名） 1000

额外奖励：
- 所有排名前 10 的队伍将获达观授予的精美参赛奖牌、证书。
- 比赛排名前 20 的选手将获得达观数据提供的全职（面向在职）和实习（面向在校生）的 VIP 通道，通过面试优先录用。（拟定）


