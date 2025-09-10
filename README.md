# HybridEffNet: Facial Expression Recognition

## HybridEffNet Backbone Summary

| Stage      | Resolution | Channels  | Notes                                    |
|------------|------------|-----------|------------------------------------------|
| Stem       | 48×48      | 32        | Conv 3×3                                 |
| Block 1–2  | 24×24      | 16–24     | Depthwise separable conv                 |
| Block 3–4  | 12×12      | 40–80     | MBConv, squeeze-and-excite               |
| Block 5–6  | 6×6        | 112–192   | Deeper MBConv, skip connections          |
| Block 7    | 3×3        | 320       | Final high-level features                |
| Head       | 1×1        | 1280      | Global avg pool + dropout + classifier   |

---

## HybridEffNet Training & Results

- **Configured Epochs:** 70  
- **Early Stop:** 61/70 (plateau)  
- **Validation Accuracy:** 0.6952  
- **PublicTest Accuracy:** 0.7040  
- **FLOPs:** 74.65 Mops  
- **Efficiency:** 934.64 (Acc% / GFLOPs)  

---

## Experiment Inventory

| Experiment        | Model  | Executed | Epochs | Best Val Acc | Test Acc | FLOPs   | Efficiency |
|-------------------|--------|----------|--------|--------------|----------|---------|------------|
| Probe Baseline    | CNN    | Yes      | —      | —            | —        | —       | —          |
| Training Baseline | CNN    | Yes      | 10     | 0.5496       | 0.55     | 0.3275  | —          |
| Training Hybrid   | EffNet | Yes      | 61     | 0.6952       | 0.70     | 0.0747  | 934.64     |

---

## Input Size Sweep

| Input Size | Test Accuracy | Efficiency (Acc% / GFLOPs) |
|------------|---------------|-----------------------------|
| 96×96      | 70.40%        | 934.64                      |
| 80×80      | 69.23%        | 1150                        |
| 104×104    | 72.26%        | 627.22                      |

---

### Interpretation
- Increasing resolution (**96 → 104**) slightly improves accuracy but reduces efficiency.  
- Reducing resolution (**96 → 80**) sacrifices some accuracy but boosts efficiency.  
- **96×96 is Pareto-optimal**, balancing accuracy and efficiency.  

---

## Reducing Computational Cost
- Compact pretrained backbone keeps FLOPs low.  
- Augmentation + calibrated loss improve accuracy at constant compute.  
- Input resolution scaling provides direct control over accuracy vs. cost trade-offs.  

**Final Result:**  
- **96×96 configuration** achieved **70.40% PublicTest accuracy**  
- **Ops:** 74,650,482  
- **Efficiency:** 934.64  

---

## Limitations
- Facial expressions in **low-resolution images** remain ambiguous.  
- Confusion between **visually similar expressions** (fear vs surprise).  
- Only two architectures explored (**ResNet, ViTs not tested**).  
- Limited ablation of augmentation strategies.  

---

## Conclusions
Within the frozen evaluation protocol, the final system combines:  
- A **pretrained EfficientNet backbone**,  
- **FER-specific augmentation**, and  
- **Calibrated loss**.  

This approach **significantly outperforms the baseline CNN** while maintaining controlled FLOPs.  

- Input-size tuning enables flexible optimisation along the **accuracy–efficiency frontier**.  

**Best Model:**  
- **HybridEffNet**  
- **70.40% PublicTest accuracy**  
- **74.65M ops**  
- **Efficiency: 934.64**  
