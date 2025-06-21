# 使用多层感知机近似数学函数的实验研究

## 摘要  
本研究探索了多层感知机（MLP）在近似六类数学函数任务上的能力，包括线性函数($y=2x$)、反比例函数($y=1/x$)、多项式函数($y=x^3$、 $y=3x^2+5$ )、复合函数($y=x+1/x$)和三角函数( $y=/sinx$ )。通过设计包含批量归一化和Dropout的正则化网络架构，并采用早停机制防止过拟合，我们在不同函数上实现了差异化的拟合效果。实验结果表明，MLP能够有效近似平滑函数，但对高阶多项式表现出较大拟合误差。本研究为神经网络函数近似提供了架构设计和正则化策略的重要参考。

## 1 引言  
函数逼近是神经网络的核心能力之一，在科学计算、工程建模等领域具有广泛应用。传统方法如多项式插值存在高震荡问题，而深度神经网络通过非线性激活函数和层次化结构，能够更稳健地逼近复杂函数。本研究系统评估了MLP在六类基础数学函数上的近似能力，重点关注以下问题：  
1. MLP对不同函数类型的逼近效果差异  
2. 正则化技术对抑制过拟合的作用  
3. 函数特性（奇点、非线性度）对模型设计的影响  

实验选取的函数覆盖了从线性到高度非线性的典型数学形式，其中 $y=1/x$ 和 $y=x+1/x$ 在 $x=0$ 处存在奇点， $y=x^3$ 和 $y=3x^2+5$ 则代表了高阶非线性关系， $y=\sin x$ 检验了周期性函数的拟合能力。

## 2 背景 

### 2.1 函数逼近理论  
根据通用逼近定理，单隐藏层MLP能以任意精度逼近紧集上的连续函数。但实际应用中，网络架构设计对逼近效率有显著影响：  
- 高度非线性函数需要更多隐藏单元  
- 存在奇点的函数需特殊数据预处理  
- 高频振荡函数需要更精细的优化策略  

### 2.2 正则化技术  
深度神经网络在函数逼近中易出现过拟合，尤其在训练数据有限时。我们集成三种正则化技术：  
1. 批量归一化：稳定各层输入的分布，加速训练  
2. Dropout：随机屏蔽神经元，增强鲁棒性  
3. 早停机制：基于验证损失终止训练，防止过拟合  

## 3 模型架构

### 3.1 网络结构  
```python
class MathFunctionNet(nn.Module):
    def __init__(self, hidden_size=128, dropout_rate=0.3):
        super().__init__()
        self.fc1 = nn.Linear(1, hidden_size)
        self.bn1 = nn.BatchNorm1d(hidden_size)
        self.dropout1 = nn.Dropout(dropout_rate)
        # 两个附加隐藏层
        self.fc2 = nn.Linear(hidden_size, hidden_size)
        self.bn2 = nn.BatchNorm1d(hidden_size)
        self.dropout2 = nn.Dropout(dropout_rate)
        self.fc3 = nn.Linear(hidden_size, hidden_size)
        self.bn3 = nn.BatchNorm1d(hidden_size)
        self.dropout3 = nn.Dropout(dropout_rate)
        self.fc4 = nn.Linear(hidden_size, 1)
        self.relu = nn.ReLU()
```
核心结构包含三个隐藏层，每层由线性变换、批量归一化、ReLU激活和Dropout组成。相比基础单隐藏层结构，深度扩展增强了非线性建模能力。

## 4 实验设置

### 4.1 数据集  
- **训练集**： $n_{\text{train}}$ 个带高斯噪声的采样点（ $\sigma=0.1-3.0$ ）  
- **验证/测试集**：各200个独立同分布样本  
- **特殊处理**：对 $y=1/x$ 和 $y=x+1/x$ 仅取 $x>0$ 区间（ $x\in[0.1,5]$ ）  

### 4.2 评估指标  
测试集均方误差：  
$$\text{MSE} = \frac{1}{n_{\text{test}}}\sum_{i=1}^{n_{\text{test}}}(y_i - \hat{y}_i)^2$$

### 4.3 超参数配置  
| 函数 | 隐藏层大小 | 学习率 | Dropout率 | 训练样本数 | 噪声 $\sigma$  | 训练轮数 |  
|------|------------|--------|-----------|------------|--------------|----------|  
| $y=2x$ | 128 | 0.005 | 0.2 | 1500 | 0.5 | 400 |  
| $y=1/x$ | 256 | 0.0008 | 0.1 | 2500 | 0.1 | 700 |  
| $y=x^3$ | 192 | 0.005 | 0.3 | 1000 | 2.0 | 500 |  
| $y=x+\frac{1}{x}$ | 256 | 0.0005 | 0.1 | 2500 | 0.2 | 700 |  
| $y=3x^2+5$ | 192 | 0.005 | 0.4 | 1500 | 3.0 | 400 |  
| $y=\sin x$ | 192 | 0.001 | 0.2 | 1000 | 0.1 | 800 |  

## 5 结果分析

### 5.1 整体性能  
| 函数 | 测试MSE | 早停轮数 |  
|------|----------|----------|  
| $y=2x$ | 0.308 | 142 |  
| $y=1/x$ | 0.131 |510 |  
| $y=x^3$ | 78.278 | 500 |  
| $y=x+\frac{1}{x}$ | 0.217 | 650 |  
| $y=3x^2+5$ | 16.917 | 400 |  
| $y=\sin x$ | 0.016 | 720 |  


### 5.2 拟合效果可视化  
![Unknown-2](https://github.com/user-attachments/assets/bb3d1436-a8e6-42d5-8853-2b5dc2ef1f22)
![Unknown-3](https://github.com/user-attachments/assets/a39fd7a3-074e-4cb6-a793-bc7076381c55)
![Unknown-4](https://github.com/user-attachments/assets/f3977f6b-df9d-4206-b870-72c00b4f013a)
![Unknown-5](https://github.com/user-attachments/assets/f0bb5038-70d8-4553-a266-2a1aab470f31)
![Unknown-6](https://github.com/user-attachments/assets/5109268b-5961-4e08-91d9-f6aa6827d072)
![Unknown](https://github.com/user-attachments/assets/fb57c676-f009-4968-90be-cb222e425f70)

## 6 结论  
本研究系统评估了MLP在数学函数逼近任务上的性能边界，得出以下结论：  
1. 架构设计原则：对平滑函数（线性、三角）可采用适中网络（128隐藏单元）；对复杂函数需增大容量（256单元）并强化正则化  
2. 正则化必要性：Dropout率与输入噪声强度正相关，高噪声场景需 $\text{dropout rate}\geq 0.4$   

## 7 附录
全部代码<br>
```python
import torch
import torch.nn as nn
import torch.optim as optim
import numpy as np
import matplotlib.pyplot as plt
from sklearn.model_selection import train_test_split
import copy

# 神经网络结构
class MathFunctionNet(nn.Module):
    def __init__(self, hidden_size=128, dropout_rate=0.3):
        super(MathFunctionNet, self).__init__()
        self.fc1 = nn.Linear(1, hidden_size)
        self.bn1 = nn.BatchNorm1d(hidden_size)  # 批量归一化
        self.dropout1 = nn.Dropout(dropout_rate)  # Dropout层

        self.fc2 = nn.Linear(hidden_size, hidden_size) # 第二个隐藏层
        self.bn2 = nn.BatchNorm1d(hidden_size) # 第二个批量归一化层
        self.dropout2 = nn.Dropout(dropout_rate) # 第二个 Dropout 层

        self.fc3 = nn.Linear(hidden_size, hidden_size) # 第三个隐藏层
        self.bn3 = nn.BatchNorm1d(hidden_size) # 第三个批量归一化层
        self.dropout3 = nn.Dropout(dropout_rate) # 第三个 Dropout 层

        self.fc4 = nn.Linear(hidden_size, 1) # 输出层
        self.relu = nn.ReLU()

    def forward(self, x):
        x = self.fc1(x)
        x = self.bn1(x)
        x = self.relu(x)
        x = self.dropout1(x)

        x = self.fc2(x)
        x = self.bn2(x)
        x = self.relu(x)
        x = self.dropout2(x)

        x = self.fc3(x)
        x = self.bn3(x)
        x = self.relu(x)
        x = self.dropout3(x)

        return self.fc4(x)

# 生成数据集
def generate_data(func, n_samples=200, noise_std=0.1, add_noise=False):
    if func == 'y=1/x' or func == 'y=x+(1/x)':
        # 只取 x > 0 的部分，避开 x=0
        x = np.linspace(0.1, 5, n_samples)
    else:
        x = np.linspace(-5, 5, n_samples)

    x = x.reshape(-1, 1)

    # 计算目标值 (理想值)
    if func == 'y=2x':
        y_ideal = 2 * x
    elif func == 'y=1/x':
        y_ideal = 1 / x
    elif func == 'y=x^3':
        y_ideal = x ** 3
    elif func == 'y=x+(1/x)':
        y_ideal = x + 1/x
    elif func == 'y=3x^2+5':
        y_ideal = 3 * x**2 + 5
    elif func == 'y=sinx':
        y_ideal = np.sin(x)

    # 添加噪声
    y = y_ideal
    if add_noise:
        noise = np.random.normal(0, noise_std, size=y_ideal.shape)
        y = y_ideal + noise

    return torch.tensor(x, dtype=torch.float32), torch.tensor(y, dtype=torch.float32)

# 训练和评估函数（含过拟合控制）
def train_and_plot(func_name, epochs=500, hidden_size=128, lr=0.005,
                   n_train_samples=1000, n_val_samples=200, n_test_samples=200,
                   noise_std=0.2, dropout_rate=0.3, weight_decay=1e-5, patience=15):
    # 生成训练数据集 (添加噪声)
    train_x_raw, train_y_raw = generate_data(
        func_name, n_samples=n_train_samples, noise_std=noise_std, add_noise=True)

    # 生成验证数据集 (添加噪声)
    val_x_raw, val_y = generate_data(
        func_name, n_samples=n_val_samples, noise_std=noise_std, add_noise=True)

    # 生成测试数据集 (添加噪声)
    test_x_raw, test_y = generate_data(
        func_name, n_samples=n_test_samples, noise_std=noise_std, add_noise=True)

    if func_name == 'y=1/x' or func_name == 'y=x+(1/x)':
        plot_x = torch.linspace(0.1, 5, 300).unsqueeze(1)
    else:
        plot_x = torch.linspace(-5, 5, 300).unsqueeze(1)

    _, y_true_plot = generate_data(func_name, n_samples=300, add_noise=False)

    # 数据预处理 - 标准化输入
    x_mean, x_std = train_x_raw.mean(), train_x_raw.std()

    train_x = (train_x_raw - x_mean) / x_std
    val_x = (val_x_raw - x_mean) / x_std
    test_x = (test_x_raw - x_mean) / x_std

    # 创建模型并移至GPU（如果可用）
    device = torch.device("cuda" if torch.cuda.is_available() else "cpu")
    model = MathFunctionNet(hidden_size=hidden_size, dropout_rate=dropout_rate).to(device)

    # 使用L2正则化（权重衰减）
    optimizer = optim.Adam(model.parameters(), lr=lr, weight_decay=weight_decay)
    criterion = nn.MSELoss()

    # 数据移至设备
    train_x, train_y = train_x.to(device), train_y_raw.to(device)
    val_x, val_y = val_x.to(device), val_y.to(device)
    test_x, test_y = test_x.to(device), test_y.to(device)

    # 训练循环（含早停机制）
    train_losses = []
    val_losses = []
    best_val_loss = float('inf')
    best_model = None
    patience_counter = 0

    for epoch in range(epochs):
        model.train()
        optimizer.zero_grad()
        outputs = model(train_x)
        loss = criterion(outputs, train_y)
        loss.backward()
        optimizer.step()
        train_losses.append(loss.item())

        # 验证阶段
        model.eval()
        with torch.no_grad():
            val_outputs = model(val_x)
            val_loss = criterion(val_outputs, val_y)
            val_losses.append(val_loss.item())

            # 早停和模型保存
            if val_loss < best_val_loss - 1e-5: 
                best_val_loss = val_loss
                patience_counter = 0
                best_model = copy.deepcopy(model.state_dict())
            else:
                patience_counter += 1

            if patience_counter >= patience:
                print(f"Early stopping at epoch {epoch+1} (best val loss: {best_val_loss:.6f})")
                break

        if (epoch+1) % 50 == 0:
            print(f'Epoch [{epoch+1}/{epochs}], Train Loss: {loss.item():.6f}, Val Loss: {val_loss.item():.6f}')

    # 加载最佳模型
    if best_model:
        model.load_state_dict(best_model)

    # 最终评估
    model.eval()
    with torch.no_grad():
        test_outputs = model(test_x)
        test_loss = criterion(test_outputs, test_y)
        print(f'Function: {func_name}, Test MSE: {test_loss.item():.6f}')

    # 生成预测曲线
    plot_x_norm = (plot_x - x_mean) / x_std
    with torch.no_grad():
        model.eval()
        plot_x_norm = plot_x_norm.to(device)
        plot_y_pred = model(plot_x_norm).cpu()

    # 将数据移回CPU以便绘图
    train_x_raw_cpu = train_x_raw.cpu()
    train_y_raw_cpu = train_y_raw.cpu()


    # 绘制结果
    plt.figure(figsize=(15, 10))


    # 函数拟合图
    plt.subplot(2, 1, 2)
    plt.scatter(train_x_raw_cpu.numpy(), train_y_raw_cpu.numpy(),
                c='blue', marker='.', alpha=0.2, label='Training Data (with noise)')
    plt.plot(plot_x.numpy(), y_true_plot.numpy(), 'r--', linewidth=3, label='True Function')
    plt.plot(plot_x.numpy(), plot_y_pred.numpy(), 'b-', linewidth=2, label='MLP Prediction')

    # 获取实际训练的 epoch 数量 (考虑到早停)
    actual_epochs = len(train_losses)

    plt.title(f'{func_name} \n MSE: {test_loss.item():.6f}, Epochs: {actual_epochs}')

    plt.xlabel('x')
    plt.ylabel('y')
    plt.legend()
    plt.grid(True)

    plt.tight_layout()
    plt.savefig(f"{func_name.replace('/', '_')}_approximation.png")
    plt.show()

    return model, test_loss.item()



# 为不同函数自定义参数
functions = [
    'y=2x',
    'y=1/x',
    'y=x^3',
    'y=x+(1/x)',
    'y=3x^2+5',
    'y=sinx'
]

results = {}
special_params = {
    'y=2x': {'noise_std': 0.5, 'lr': 0.005, 'n_train_samples': 1500, 'epochs': 400, 'patience': 25, 'hidden_size': 128, 'dropout_rate': 0.2}, # 适度调整
    'y=1/x': {'hidden_size': 256, 'noise_std': 0.1, 'n_train_samples': 2500, 'lr': 0.0008, 'epochs': 700, 'patience': 40, 'dropout_rate': 0.1}, # 增加层数后降低 hidden_size，适度增加样本，调整 lr, patience
    'y=x^3': {'noise_std': 2.0, 'dropout_rate': 0.3, 'epochs': 500, 'patience': 30, 'hidden_size': 192}, # 适度调整
    'y=x+(1/x)': {'noise_std': 0.2, 'lr': 0.0005, 'hidden_size': 256, 'n_train_samples': 2500, 'epochs': 700, 'patience': 40, 'dropout_rate': 0.1}, # 增加层数后降低 hidden_size，适度增加样本，调整 lr, patience
    'y=3x^2+5': {'noise_std': 3.0, 'dropout_rate': 0.4, 'n_train_samples': 1500, 'epochs': 400, 'patience': 25, 'hidden_size': 192}, # 适度调整
    'y=sinx': {'hidden_size': 192, 'noise_std': 0.1, 'lr': 0.001, 'epochs': 800, 'patience': 40, 'dropout_rate': 0.2} # 适度调整
}

for func in functions:
    print(f"\n{'='*70}")
    print(f"=== Fitting {func} ===")
    print(f"{'='*70}")

    # 基本参数
    params = {
        'func_name': func,
        'epochs': 300, # 降低基础 epochs
        'hidden_size': 128, # 基础 hidden_size
        'lr': 0.005, # 基础 lr
        'dropout_rate': 0.3, # 基础 dropout
        'weight_decay': 1e-5, # 基础 weight_decay
        'patience': 20, # 基础 patience
        'n_train_samples': 1000, # 基础训练样本数
        'n_val_samples': 200, # 基础验证样本数
        'n_test_samples': 200 # 基础测试样本数
    }

    # 应用特定参数，确保覆盖基础参数
    params.update(special_params.get(func, {}))

    model, mse = train_and_plot(**params)
    results[func] = mse

# 显示所有结果
print("\n最终结果汇总:")
for func, mse in results.items():
    print(f"{func}: 测试MSE = {mse:.6f}")
```
