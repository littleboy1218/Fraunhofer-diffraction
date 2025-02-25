# MATLAB 夫琅禾费衍射仿真 GUI


![矩孔衍射](https://i-blog.csdnimg.cn/direct/7e7fadaadc714165915c43284f439ed5.png#pic_center)
![圆孔](https://i-blog.csdnimg.cn/direct/991e08546e7741a7b52ad3552f103054.png#pic_center)
![双缝](https://i-blog.csdnimg.cn/direct/231c9436461c479ba6a809409c3141bc.png#pic_center)
![单缝](https://i-blog.csdnimg.cn/direct/003692bbd0844f4fabd423021762040c.png#pic_center)

![单缝_2D强度，点击3D即可切换到2D](https://i-blog.csdnimg.cn/direct/ee03bf9353f64595b891cab3a8159fa6.png#pic_center)

基于MATLAB App Designer开发的交互式夫琅禾费衍射仿真工具，支持实时参数调节与多视图可视化。



## 主要功能
- **四种衍射模式**:
  - 🟦 矩形孔衍射（宽/高独立调节）
  - ⚪ 圆孔衍射（直径调节）
  - ░ 单缝衍射（缝宽调节）
  - ░░ 双缝衍射（缝宽/间距调节）
- **动态参数控制**:
  - 🌈 波长调节（300-800nm连续可调）
  - 📏 衍射距离（0-1000mm）
  - 🎚️ 孔径尺寸（0.001-8mm）
  - 🎯 孔径中心位置调节（X/Y方向）
- **可视化功能**:
  - 2D强度分布图（灰度映射）（点击3D即可切换到2D）
  - 3D强度曲面图（Jet色彩映射）
  - 实时坐标系同步更新

## 快速入门
### 环境要求
- MATLAB R2020b 或更新版本
- 必需工具箱：
  - MATLAB App Designer
  - Image Processing Toolbox（用于图像显示）

### 安装步骤
1. 克隆仓库或下载`.mlapp`文件：
   ```bash
   git clone https://github.com/yourusername/diffraction-simulator.git
   ```
2. 启动MATLAB并导航至项目目录

### 运行应用程序
- **方法1**：直接运行（推荐）
  ```matlab
   >> appdesigner('Rectangular_5.mlapp');  % 在编辑器中打开
   ```
   
- **方法2**：编译运行（需安装MATLAB Compiler）
  ```matlab
   >> app = Rectangular_5;
   >> app.run();  % 直接启动独立应用
   ```

## 界面操作指南
| 面板区域        | 功能说明                               |
|-----------------|----------------------------------------|
| 🔧 参数调节区（左） | - 下拉菜单选择孔径类型<br>- 旋钮/滑块调节参数 |
| 📊 显示区（右）    | - 上：衍射图案二维显示<br>- 下：三维强度分布 |

**典型操作流程**：
1. 通过下拉菜单选择孔径类型
2. 使用中央旋钮调节波长（635nm对应红光）
3. 拖动右侧滑块调整孔径尺寸
4. 观察衍射图案的实时变化
5. 点击3D视图进行视角旋转（支持鼠标拖拽）

## 项目结构
```
Fraunhofer-Diffraction-Simulator/
├── Rectangular_5.mlapp       # 主应用程序文件
├── Resources/                # 图标资源
│   ├── aperture_icons/       # 孔径类型图标
│   └── preview_images/       # 示例图像
├── Documentation/            # 理论文档
│   ├── Fraunhofer_Theory.pdf # 衍射理论
│   └── User_Manual.pdf       # 详细使用手册
└── LICENSE                   # MIT许可证文件
```

## 开发扩展
### 核心算法
```matlab
% 夫琅禾费衍射计算核心（部分代码）
function Fraunhofer(app)
    N = 300; % 固定采样点数
    lambda = app.Wave.Value*1e-6; % 单位转换(mm→m)
    
    % 生成坐标网格
    [X,Y] = meshgrid(linspace(-app.wide2/2, app.wide2/2, N));
    
    % 不同孔径类型的衍射计算
    switch app.flag
        case 1 % 矩形孔
            I = (sinc(app.Delta_x*X/(lambda*app.d)).^2) .* ...
                (sinc(app.Delta_y*Y/(lambda*app.d)).^2);
        case 2 % 单缝
            % ...（类似计算）...
    end
    
    % 归一化显示
    imagesc(app.UIAxes2, I/max(I(:)));
end
```

### 扩展建议
1. **新增孔径类型**：
   - 修改`DropDown.Items`属性添加新选项
   - 在`DropDownValueChanged`回调中补充计算逻辑

2. **添加数据导出**：
   ```matlab
   % 在右侧面板添加按钮回调函数
   function exportButtonPushed(app, event)
       imwrite(getframe(app.UIAxes2).cdata, 'diffraction_pattern.png');
   end
   ```

## 常见问题
❓ **无法启动应用程序**  
✅ 确认：
- 使用支持App Designer的MATLAB版本
- Image Processing Toolbox已安装
- 文件路径不含中文或特殊字符

❓ **3D视图显示异常**  
✅ 尝试：
```matlab
app.UIAxes3.View = [30, 30];  % 重置视角
axis(app.UIAxes3, 'equal');   % 修复比例失真
```

## 贡献指南
欢迎通过以下方式参与改进：
1. 提交Issue报告问题或建议
2. Fork仓库并创建Pull Request
3. 完善文档或添加测试案例

**维护团队**：  
[您的名字]  
📧 [contact](zp20021218@163.com)  
🌐 [项目主页](https://github.com/littleboy1218/Fraunhofer-diffraction)

---

📜 **许可证**  
本项目采用 [MIT License](LICENSE)，允许自由使用、修改和分发，请保留原始版权声明。

