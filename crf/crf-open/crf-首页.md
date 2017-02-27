

#文档首页 
原文档：http://crf.sourceforge.net/introduction/

- Overview
- Using CRF for Sequential Learning
- Various Interfaces
- Features, Feature Types and Feature Generator
- Models
- Configuration Parameters
- References

## Overview

build/	:	 存储所有的编译的java class文件 
doc/	:	Javadoc and this documentation for the package  
samples/	:	 数据集合对应的配置文件
src/	:	Java source files  原文件
lib/	:	jar files  jar包

安装1.4以上版本的jdk，和安装ant。

An example use of the package is provided as sample code; 
it gives an application of the CRF package to a text segmentation task. 
示例包的示例使用提供示例代码；
它给出了一个应用程序的CRF包的文本分割任务。

This example uses CRF to segment a string or text into predefined fields or attributes.
此示例使用CRF将字符串或文本分割为预定义的字段或属性
The code for the application can be found in src/iitb/Segment directory which demonstrates 
implementation of various interfaces needed to use the package.
该应用程序的代码中可以找到src /学/段目录说明
实现使用包所需的各种接口

A sample dataset is given in the samples/ directory, 
along with the configuration file for the same. 
样本数据集在 samples/目录中给出，配置文件也在这里。

The training and test sets are US addresses, 
which are required to be segmented into constituent fields (as given in the training set). 
The instructions to run the application are given in the README file.
训练和测试集是美国地址，
这是需要分割成组成字段（给定的训练集）。
运行应用程序的指令在自述文件了。

The code of the distribution is organized into various packages. 分配的代码被组织成各种包。
The source code can be found in the src/ directory of the distribution. 
A summary of various packages is given below.下面给出了各种软件包的摘要。

iitb.CRF	:	Core package; contains implementation of training and inferencing algorithms and defines various interfaces to be implemented by the user of the distribution.
包含训练和推理算法实现了由用户实现各种接口分布

iitb.Model	:	Stores implementation of various graphs, features, and feature generator (see next section).
商店的各种图表，功能和功能生成器（见下一节）。

iitb.Utils	:	Common classes used by other packages.
iitb.MaxentClassifier	:	An application of CRF to a maximum entropy based classification task.
CRF在最大熵分类任务中的应用。
iitb.Segment	:	An application of CRF to a text segmentation task.
CRF在文本分割任务中的应用。



## Using CRF for Sequential Learning
使用CRF持续学习
Training and inferencing are the two essential components of a learning task. 
培训和推理是学习任务的两个不可或缺的组成部分
A labeled set of examples (referred to as training set in literature), 
一组标记的例子（简称为文献中的训练集）
is used in case of supervised learning to learn the parameters of the model. 
用于监督学习的情况下，学习模型的参数
The training algorithm learns a model using the training set, 
训练算法学习模型使用训练集，
which can then be applied to an unseen instance to predict the property for which the model is learned. 
然后，它可以被施加到一个看不见的实例来预测的属性，该模型据悉。
This process of predicting the property for an unseen instance using the learned model is referred to as inferencing.
这一预测的属性为一个看不见的实例，运用所学的模型被称为推理过程。

A sequential learning task differs from a conventional record based learning application in that the basic data instance (x) here is a sequence of tokens (x_i, i = 1,2... n, n = |x|), 
序列学习任务不同于传统的基于记录的学习应用在基本数据实例（x）这是一个标记序列（x_i，i = 1,2…N，N = | X |），
and the task is to assign a label (from some predefined set of values) to each of these tokens. 
任务是给每个标记分配一个标签（从一些预定义的值集）。
Thus, the task here is to predict the label sequence y for the token sequence x. 
因此，这里的任务是预测的标签序列Y的令牌序列X。
A particular label assigned to a token may depend upon the position of the token (i for x_i) in the sequence, 
一个被分配到一个特定的标签可能取决于令牌的令牌的位置（我x_i）序列中的，
as well as the labels assigned to previous token(s) (Markov property). 
以及分配给以前的令牌（S）（马尔可夫财产）的标签。
CRFs, being conditional models, CRFs，是有条件的模型，

as opposed to generative models like HMMs, 而不是像HMM模型生成模型，
allow large range dependencies on x values in the sequence without making the inferencing problem intractable. 
让X值大范围依赖序列中没有推理的棘手问题。
These large range x dependencies are encoded by functions defined over the history for the current token. 
这些大范围X依赖性的编码功能定义的历史上的当前令牌。

History for a token captures the characteristics of the tokens within a window around the current token, 
令牌的历史在当前令牌周围的窗口内捕获令牌的特性，
current label (y) in consideration, and optionally previous	y value(s). 当前标签（Y）的考虑，以及可选的Y值（s）。
These functions are known as features.这些函数被称为特征。

To use CRF for a sequential learning task, you should have a training set. 
要使用CRF的顺序学习任务，你应该有一个训练集。
Also, you would like to specify various features to encode various dependencies for the particular task at hand. 
此外，您还需要指定各种功能来对当前特定任务的各种依赖项进行编码
The training set, as described above, will be used to learn a model. 
训练集，如上所述，将被用来学习一个模型。
The learned model, here, is a set of parameters (known as weights) 
这里所学的模型是一组参数（称为权重）
which corresponds to the importance of the features used in the task 
它对应于在任务中使用的功能的重要性
(more details about the learning process can be obtained from the references).
（关于学习过程的更多细节可以从参考文献中获得）。


The core component of the package are the learning and inferencing algorithms. The learning algorithm expects a DataIter interface to access the training set. Also, each training or test instance is required to be accessed through the DataSequence interface. Thus, you will need to encapsulate your single sequential data instance into a class which implements the DataSequence interface. These are the basic interface that you will need to create; details given in next section.

The package also implements some of the most commonly used features in a typical sequential learning task. Details about the implemented features can be found in the Features section. However, based on your application domain, you may want to add new features to the learning task. The package has the flexibility to define new features through the provided interfaces related to features.

The learning and inferencing algorithms use a feature generator to access various features used for the task. The interface for feature generator is defined in FeatureGenerator. The package contains an implementation (FeatureGenImpl) of the FeatureGenerator interface. For most users, FeatureGenImpl class with the default features would suffice for their tasks. An advance user, who needs to create and add his/her own features, would need to extend this FeatureGenImpl class or implement a new feature generator class depending on his/her requirements. The Feature interface defines a feature, a sample implementation of which is the FeatureImpl class provided with the package. The package also defines an abstract class FeatureTypes which encapsulates a set of similar features. To add new features, you would need to extend this FeatureTypes class. Once you implement new features by extending FeatureTypes class, you would want to add it to a feature generator class so that the new features are used for the learning task. More details about how to create new features and how to add them to a feature generator can be found in the Various Interfaces and Features sections.

For sequential learning problems, it becomes essential to encode transition from one label type to another. Also, for some tasks, a single entity to be labeled may consist of a sequence of tokens. For example, in the address segmentation task, we may need to encode the fact that with high probability the area name would be followed by a city name. Also, an area name may consist of more than one word. The package supports encoding of such transitions as well as of multiple tokens per label by allowing a user to input a graph of labels representing various relationships. More details about this input parameter can be found in the Models section.


### Steps for Using CRF for Sequential Learning
The following are the steps to train a model using the CRF:
以下是使用CRF训练模型的步骤：
- Create an instance of the feature generator class -- FeatureGenImpl or a new feature generator that you might have created.
创建的特征生成类的一个实例——featuregenimpl或新功能的发电机，您可能已经创建
Also, pass the required parameters like the modelGraph value (see Models section) and number of labels in the label set, to the constructor.
同时，通过规定的参数如modelgraph值（见模型部分）和数量的标签中设置标签的构造函数。
- Create an instance of the iitb.CRF class. 创建的iitb.crf类的一个实例。
This class is the interface for the training and inference routine of the package. 
这个类是包的训练和推理例程的接口。
You will need to pass the object of the feature generator just created as one of the parameters to the constructor of the iitb.CRF class.
你将需要通过对象的特征作为发电机刚刚创建的iitb.crf类的构造函数的参数之一。
- Read the instances of the training set in the objects of the class implementing the DataSequence interface.
读取设置在类实现接口的对象，拓宽培训的情况。
Also, create an object of the class implementing the DataIter interface to encapsulate these data sequences.
同时，创建类的实现dataiter接口来封装这些数据序列的对象。
- Call the train routine of the feature generator class to train the dictionary 
调用特征生成器类的训练例程来训练字典
(i.e. to construct on-the-fly dictionary for creating dictionary features; see FeatureTypes for details). 
（即构建上飞字典创建词典的功能；看到FeatureTypes的详情）。
Pass the DataIter object to the train routine.
通过dataiter对象火车常规。
- Call the train routine of the CRF object. 
调用CRF对象的列车例行程序。
Again, you will have to pass the training set iterator object to this train routine.
同样，您必须将训练集迭代器对象传递给该例程。

Once the model is learnt from the training set, 一旦模型从训练集学习，
you can apply it to an unseen or test instance by calling the apply() method of the iitb.CRF class. 
你可以将它应用到一个看不见的或测试通过调用类的iitb.crf apply()方法实例。
You can save the model by calling the write() methods of both the iitb.CRF class as well as the feature generator, 
你可以通过调用类的iitb.crf write()方法以及特征生成器保存模型
so as to be able to use it later for inferencing. 
以便能使用它后的推理。
You can use the read() methods of these classes to retrieve the parameters of a previously learnt model.
你可以使用这些类的read()方法来检索一个以前学过的模型参数。

A template for implementing your own CRF application using this package can be found here.
一个模板，实现自己的CRF应用程序使用此包可以在这里找到。
http://crf.sourceforge.net/introduction/CRFAppl.java


##Various Interfaces   各种接口





