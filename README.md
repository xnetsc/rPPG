# rPPG 实时生理信号检测

纯前端实现，单个 HTML 文件，零依赖安装。直接用浏览器打开即可运行。

## 功能

| 指标 | 方法 |
|------|------|
| 心率 (bpm) | POS / CHROM / 绿通道算法 + FFT 峰值 |
| 血氧 SpO₂ | RGB 红/蓝 AC-DC 比值法（估算，非医疗级） |
| HRV RMSSD | 脉搏波峰间期标准差（估算） |
| 信号质量 SNR | 频谱信噪比 (dB) |

## 原理

rPPG (Remote Photoplethysmography) 利用摄像头捕捉皮肤表面微弱的颜色变化来推算脉搏：

```
视频帧 → 人脸检测 → 前额 ROI → 逐帧 RGB 均值
      → POS/CHROM 算法 → 脉搏信号
      → 带通滤波 (0.7–4 Hz) → FFT 峰值 → 心率 bpm
```

**POS 算法**（Wang et al., 2017）：
- 将 RGB 归一化消除光照变化
- 投影到皮肤色调正交平面提取脉搏分量
- S = (R−G) + α(R+G−2B)，其中 α = σ(S1)/σ(S2)

## 使用

```bash
# 方法1：直接在浏览器打开
open index.html

# 方法2：本地 HTTP 服务（推荐，部分浏览器需要 HTTPS 才能访问摄像头）
python3 -m http.server 8080
# 然后访问 http://localhost:8080
```

## 注意事项

- 需要 HTTPS 或 localhost（浏览器摄像头权限限制）
- 建议光线均匀、减少移动、采集约 10 秒后心率趋于稳定
- 所有计算在本地浏览器完成，不上传任何数据
- **本应用仅供演示，数据不具医疗意义**

## 参考文献

- Wang, W. et al. (2017). Algorithmic Principles of Remote PPG. *IEEE TBME*, 64(7).
- de Haan, G. & Jeanne, V. (2013). Robust Pulse Rate From Chrominance-Based rPPG. *IEEE TBME*, 60(10).
