## My Pytorch Learning Note
### Note1：CNN image classification

Welcome to my site.I'll use Cinese to write this note.If you have any question, welcome email me for discuss.  
My email: wangyuanhang94@gmail.com

#### 1、模型训练报错：RuntimeError: CUDA out of memory. Tried to allocate 128.00 MiB

程序中直接导入训练好的模型，不需要再次进行反向传播。因此在进行前向传播之前加入或者在出现错误的地方加上：
```
with torch.no_grad():
```
即
```
with torch.no_grad():
    train_pred = model(data[0].cuda())  # 调用forward
```
详见代码CNN.py

#### 2、模型训练报错：RuntimeError: element 0 of tensors does not require grad and does not have a grad_fn

出现这种错误是因为，构建Variable, 要注意得传入一个参数requires_grad=True, 这个参数表示是否对这个变量求梯度， 默认的是False, 也就是不对这个变量求梯度，故应在求反向传播前加入requires_grad设为True，如下：
```
batch_loss = loss(train_pred, data[1].cuda())
batch_loss.requires_grad = True
batch_loss.backward()
```
