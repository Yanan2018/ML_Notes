# Tips for Deep Learning

> 本节课讲几个Deep learning的tips.这节课本来要在CNN之前讲，在CNN那段里面，我们留下了两个问题，第一个是，在CNN里面，有max pooling这样的架构，但是，max pooling这样的架构显然不能微分，如果放在nerual network里面，在做Gradient Descent时，在微分的时候，到底怎么处理？第二个问题，是我们刚才看到的，L1的Regularization,但是我们还没有解释它是什么东西。我们都会在本节课中解释。

## Recipe of Deep Learning

最重要的一个观念是，Deep learning的recipe.即在训练一个Deep Learning的network的时候，它的流程应该是什么样子的。

我们都知道Deep Learning的三个Steps，define function, define function set, define the structure of the network, 决定你的loss function, 接下来就可以用Gradient Descent去做optimization, 做完这些事情以后，你会得到一个nerual network.那么，接下来，要做什么样的事情呢？

第一件要检查的事情就是，这个neural network在你的training set上，有没有得到好的结果？不是在testing set上，你要先检查这个nerual network, 在training set上，有没有得到好的结果，如果没有的话，就需要回头去看看，在这3个Step里面，是不是哪里出了问题。可以做什么样的修改，让你在training set上，能够得到好的结果。__那这边先检查training set的performance__,其实是Deep Learning一个非常unique的地方。如果你想想看其他的方法，比如说，你今天如果用的是k nearest neighbor，或者decision tree, 这些方法你做完以后，你其实会不太想检查你的training set的结果。因为在training set上的正确率就是100.没有什么检查的必要。所以有人说，deep leaerning,看模型里有这么多的参数，感觉很容易overfitting，但实际上，这个Deep learning的方法，非常不容易overfitting。因为我们说的overfitting就是在training set上performance很好，但在testing set上performance没有那么好。像knn,decision tree, 它们一做下去，在training set上的正确率都是100,这个才会非常容易overfitting.不是说Deep Learning没有overfitting的问题，而是说，在Deep Learning里面，overfitting不是第一个你会遇到的问题。__你第一个会遇到的问题，__ 是你在training的时候，它并不像k nearest neighbor这种方法一样，你一train就可以得到非常好的正确率，它有可能在training set上，__根本无法给你一个好的正确率。__ 所以，这个时候，要回头去检查，在前面的step里面，要做什么样的修改，好让你在training set上得到好的正确率。假设你现在已经很幸运地在training set上得到了的performance, 你要用deep learning在training set上得到100%的正确率是没有那么容易的，但可能你在MNIST上得到一个99.8%的正确率，接下来，把你的network apply到testing set上，testing上的performance才是我们最后真正关心的performance.那么，在testing set上的performance怎么样呢？如果现在得到的结果是NO的话，那么就是overfitting.这个情况才是overfitting.你在training set上得到好的结果，但是在testing set上得到的是不好的结果，这个时候，这种情况，才叫做overfitting.那你要回头过去，做某一些事情，试着去解决overfitting这个问题。有时候，你试着加了新的tech,试着去解决overfitting这个问题的时候，你其实会让training set上的结果变坏，所以，在做这一步的修改以后，你要先回头去检查说，training set上的结果，是怎么样的。如果Training Set上的结果变坏的话，你就需要从头对你的network training的process做一些调整。如果你同时在training set和testing set上都得到好的结果的话，最后，就可以把你的系统真正用在application上。这个结论就是：不要看到任何不好的performance都说是overfitting.

![image.png](attachment:image.png)

## Do not always blame Overfitting

Below is an example - 

横坐标，是model参数update的次数，纵坐标是error rate,所以越低越好。如果我们现在比较一个20层的network,和一个56层的network,你会发现56层的network,error rate比较高，performance比较差，20层的newtork的performance是比较好的。有些人看到这个图，马上就会得到一个结论：说56层参数太多了，56层是没有必要的。这个是overfitting. 但是，真的是overfitting吗？

在得出这个结论之前，要先检查一下在training set上的结果，对某些方法来说，你不用检查这件事，比如KNN或者Decision tree,但是对nerual network来说，你是需要检查这件事情的。为什么呢？因为有可能你在training set上得到的结果，20层的nerual network的performance本来就比56层的要好。在training set上，56层的neural network它的performance是比较差的。为什么会这样呢？因为在nerual network training的时候，有太多的问题，会让你的training的结果不好。比如说，我们有local minimum的问题，有Saddle point的问题，有plateau的问题，有种种问题，所以有可能这个56层的nerual network,在train的时候，卡在了一个local minimum的地方。所以它得到了一个比较差的参数。因此，它并不是一个overfitting.而是在train的时候，并没有train好。

这个也不叫underfitting.Underfitting指的是model的复杂度不够，参数不够多，模型的能力不足以解决这个问题。因此这个56层的network比20层的performance差，并不是因为它能力不够，而是没有train好的结果。

![image.png](attachment:image.png)

因此，在deep learning中，如果看到了一个方法，永远要想一下，这个方法是要解决什么问题。因为在Deep learning中，有两个问题，一个是training set上的performance不好，一个是testing set上的performance不好。那么这个方法应该是针对这两个问题中的一个来做处理。

以dropout方法为例，我看到performance不好，我就用Dropout。但是，你要仔细想一下，dropout是什么时候用的。Dropout是你在testing的结果不好的时候，你才会用apply dropout.你的testing的结果好的时候，你是不会用dropout的。如果你是因为training set结果不好引起的，你采用dropout，会导致越train越差。所以，不同的方法，针对不同的问题。

![image.png](attachment:image.png)

## Recipt of deep learning - Different approaches for different problems

针对不同的问题，分开来讨论。

![image.png](attachment:image.png)

### 情况1： 在training set上的perforamance比较差

如果我们在training set上得到的结果比较差，那我们就需要考虑，是不是我们在做network架构设计的时候，没有设计好。举例来说，你可能用的是activation function, 是比较不好的activation function.你可能会换一些新的activation function.它可以给你比较好的结果。

比如下面这个例子。当layer到9/10的时候，模型的表现非常差，有的人会说：这是overfitting,因为9/10层的参数太多。但其实这样的判断是不正确的。因为我们需要先去检查，这个performance不好，是不是来自于overfitting,要先去看training set的结果。

![image.png](attachment:image.png)

这个结果实际上是在training Set上的结果。

![image.png](attachment:image.png)

### Training set performance is bad - Reason 01: Vanishing Gradient Problem

#### What is `Vanishing Gradient Problem`

Training set表现不好的原因，有可能的之一是 Vanishing Gradient Problem. 当network叠的很深的时候，在最靠近input的地方，参数对loss function的微分会是很小的，但是在比较靠近output的地方，参数对loss function的微分值反而比较大。因此，当你设定同样的learning rate的时候，你会发现，在靠近input的地方，参数的update非常慢，靠近output的地方，参数的update非常快。因此，造成的结果就是，在input还继续是random的时候，outout已经根据random的结果找到了一个local minimum,converge了。

![image.png](attachment:image.png)

#### Why have `Vanishing Gradient Problem`?

Sigmoid function会导致Vanishing Gradient Problem. 










```python

```


```python

```


```python

```


```python

```
