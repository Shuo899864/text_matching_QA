# 构建问答系统
## 一、项目背景介绍
问答系统(Question Answering System, QA)是信息检索系统的一种高级形式，它能用准确、简洁的自然语言回答用户用自然语言提出的问题。问答系统在搜索引擎、智能客服和智能助手等应用场景中都发挥着重要作用。检索式问答系统是问答系统的重要类别，它能从大量文本中检索出问题的答案。
## 二、数据介绍
本项目使用了 baike_qa2019 百科类问答json版 数据集和lcqmc文本匹配数据集，其中百科问答数据集中包含100万条问答对，可以作为问答系统的检索库，lcqmc数据集则是用来训练一个检索模型。
## 三、模型介绍
本项目只实现了检索式问答中粗排的召回模型，使用sentence transformer模型，sentence transformer是双塔的模型结构，query和title两个句子分别输入同一个ernie模型，得到编码后的embedding，然后进行pooling操作，输出分别记作u，v。之后将三个表征（u,v,|u-v|)拼接起来，输入线性层进行二分类。 （此处有图） 使用双塔模型的好处是可以在推理阶段将大规模问答库中的海量title提前编码成embedding，然后建立索引，在召回阶段只需要计算query的embedding，然后使用索引计算向量相似度，可以加快速度。
## 四、模型训练
首先定义一个输入转换函数convert_example，它将dataset单条数据转换成token_id。设置batch_size为128，加载ernie-tiny预训练模型的分词器，然后用DataLoader加载训练集和验证集。
## 五、模型评估
训练完成后，我们还要定义一个模型，用来把单条query或title编码成embedding。
## 六、总结与升华
项目暂时只实现了point-wise的召回模型，虽然配合索引计算速度比较快，但是精度有限，双塔结构的模型忽略了文本对之间的信息交互，如果在召回之后使用pair-wise的预训练模型进行精排，效果应该更好。
## 七、个人总结
我是一个深度学习小白，主要感兴趣的方向为NLP问答、对话、文本生成等。
