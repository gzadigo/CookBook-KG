# CookBook-KG
A knowledge graph for Chinese cookbook（中式菜谱知识图谱），可以实现知识图谱可视化和知识库智能问答系统（KBQA)  

本项目开发的系统名称为**AI Food Time**，中文名为**爱食光**。如需体验可视化功能可直接访问[**GitHub Page**](https://ngl567.github.io/CookBook-KG/)。  
通过收集网上完全公开的有关中式菜谱的数据，经过**数据清洗和分析**，转换为**知识图谱的存储结构**，并提供**可视化展示与搜索**和**智能问答**等功能，为热爱美食与烹饪的人们提供方便快捷的中式菜谱服务，并以知识图谱的形式直观显示出不同菜品的关系及所用原料，在生活中具有很大的实际应用需求，包括：
+ 一类菜品的不同具体做法，例如水煮鱼包括麻辣水煮鱼、小清新版水煮鱼和家常版水煮鱼等；
+ 通过菜品与食材的关联关系，可以查询家中现有食材可以烹饪哪些菜品；
+ 可以直接显示出每种菜品所需主料，辅料，配料及其具体数量和烹饪方法，与网上的一些菜谱网页相比更加简单直观；
+ 可视化能够对各种菜品及关联关系有一个全局的认识；
+ 智能问答系统可采用自然语言进行提问，系统反馈答案结果。
## 功能使用
### 可视化展示及搜索：
![image](https://github.com/ngl567/CookBook-KG/blob/master/miniviz-1.png)  ![image](https://github.com/ngl567/CookBook-KG/blob/master/miniviz-2-fig.png)  
同一类实体用相同颜色的节点表示，鼠标位于某个节点上方时显示其相关联的其它实体和之间的关系名称；  
具有同一类实体显示开关，节点显示模式转换，并支持搜索功能；  
每种菜品的信息栏中显示菜品对应的成品图片，并利用entities_aglin.py进行了实体对齐，消除了食品原料中的冗余信息。
+ **mini**版：包含10大类，**50**种菜品之间的关联关系，包括菜品制作的各种食材和制作步骤，轻量级的mini版同时支持电脑和手机浏览器打开，如需体验可直接进入Github Page[**访问入口**](https://ngl567.github.io/CookBook-KG/)。
+ **pro**版(开发中)：包含**362**大类，**八千多**种菜品之间的关联关系，包括菜品制作的各种原料和制作步骤。

### 智能问答系统（KBQA）：
![image](https://github.com/ngl567/CookBook-KG/blob/master/kbqa.png)  
基于构建的中式菜谱知识图谱，针对其中和菜品有关的各类问题，智能问答系统可以给出对应问题的答案。  
本项目中的智能问答机器人名为**小吃**。  
使用本系统需要预装软件：  
+ Apache Jena Fuseki：Jena Fuseki是一个SPARQL服务，通过HTTP提供使用SPARQL协议的REST式SPARQLHTTP更新，SPARQL查询和SPARQL更新。  
从[**官网**](http://jena.apache.org/download/)下载最新版本的fuseki压缩包，并解压到目标文件夹。在apache-jena-fuseki的目标文件夹下用命令行输入命令`java -jar fuseki-server.jar`，启动Fuseki服务。接着，打开浏览器，访问：<http://localhost:3030>，创建一个持久化数据库，并上传/data/aifoodtime_ntriples.nt三元组数据集，完成知识库的准备。
+ JAVA：运行fuseki需要java环境，如果没有安装JAVA8.0及以上版本，请前往[oracle官网](http://www.oracle.com/technetwork/java/javase/downloads/index.html)上下载最新版本的JDK然后安装，并配置环境路径。
系统的流程为：解析输入的自然语言问句生成 SPARQL 查询，进一步请求后台基于 TDB 知识库的 Apache Jena Fuseki 服务, 得到答案。如果知识库中不存在问题的答案或者对于提出的自然语言问题无法理解，系统也会给出相应回复。
#### 可以提问的问题类型：
&nbsp;&nbsp;1.某一类菜包含的具体菜品；  
&nbsp;&nbsp;2.某一个特色菜品的所有原料；  
&nbsp;&nbsp;3.某一个特色菜品的主料，辅料和配料；  
&nbsp;&nbsp;4.某一个特色菜品的特点；  
&nbsp;&nbsp;5.某一个特色菜品的制作步骤。
#### 使用方法：  
在已经启动Fuseki服务的情况下，命令行输入`python query_main.py`，就可以启动问答系统，开始问答过程：
```
cd KBQA
python query_main.py
```
**问答示例1：**  
```
请提问：
水煮鱼类包括哪些菜？

小吃：
家常水煮鱼、小清新版水煮鱼、水煮鱼、香辣水煮鱼、麻辣水煮鱼
```
**问答示例2：**  
```
请提问：
如何制作水煮鱼？

小吃：
1: 准备食材。2: 将鱼清洗干净后切片，鱼骨和鱼肉分开放。黄豆芽去掉须根、辣椒剪成段、姜切片。接下来开始腌鱼。鱼骨中放入三四片姜、一勺料酒、半勺盐腌制二十分钟。鱼片中放入半只蛋清、一勺料酒、一勺淀粉、一小勺白胡椒粉后抓匀腌制二十分钟。 3: 锅中加适量清水，水烧开后将黄豆芽放入锅中，再放少许盐。待黄豆芽煮熟捞出放入大碗内。4: 锅中倒入少许油，油热后放入一勺郫县豆瓣酱和姜片煸炒出红油（喜欢更辣一点口感的，可以往锅里再放一些干辣椒煸炒出香味）。5: 往锅里倒入适量的热水。6: 将鱼骨放入锅内，大火烧开后转小火炖10分钟。（这个时候可以尝一下鱼汤的味道，然后根据自己的口味选择要不要放盐）鱼骨炖好后捞到装有黄豆芽的大碗中。7: 将鱼肉一片一片的放入锅中。（千万不要搅动，如需要，只要轻轻晃动几下锅即可）8: 煮开后立即将鱼肉捞出，再盛一些汤到碗中。9: 换一只干净的锅中，锅中倒入40毫升左右的食用油，然后将干辣椒和花椒放入锅中，小火煸炒至辣椒红亮，花椒出香味即可捞出放到鱼肉上。10: 将锅中剩下的油大火烧至稍稍冒烟，然后关火，把油淋在鱼肉上即可。
```
**问答示例3：**  
```
请提问：
鱼香肉丝的特点是什么？

小吃：
难度: 简单、耗时: 二十分钟、口味: 鱼香、工艺: 炒
```
**问答示例4：**  
```
请提问：
山楂红烧肉的主料具体都是哪些啊？

小吃：
香叶: 3片、五花肉: 400克、八角: 2个、冰糖: 20克、姜: 8克、山楂: 200克、桂皮: 4克、花椒: 20颗
```
**问答示例5：**  
```
请提问：
可乐鸡翅需要哪些食材？

小吃：
料酒、姜、葱、八角、可乐、鸡翅中
```
**问答示例6：**  
```
请提问：
水煮肉片的辅料是什么？

小吃：
油菜: 适量、绿豆芽: 适量、金针菇: 适量
```
**问答示例7：**  
```
请提问：
今天天气如何？

小吃：
这个问题我真是无法回答。
```
