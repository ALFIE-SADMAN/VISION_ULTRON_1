
 Competition Name: Facial Expression Recognition/Classification
 Final Report
 Introduction (task & baseline). We address 7-class facial-expression classification on FER2013 (48×48
 grayscale). The course notebook fixes the split and metric code: Training for learning, PrivateTest for
 validation only, and PublicTest for the final test only. We leave those components untouched and modify
 only the model, loss/regularisation, augmentation, and optimiser/schedule.
 Baseline Model.
 The baseline model is a plain CNN designed specifically for FER2013. It is structured as three 3×3-ReLU
 convolutional blocks followed by max-pooling, flattening, and a deep multi-layer perceptron (MLP) head.
 The detailed architecture is shown below:
 Layer
 Kernel/Units
 Activation Output Size (approx.)
 Conv2D + MaxPool 3×3, 32 filters ReLU
 46×46×32 → 23×23×32
 Conv2D + MaxPool 3×3, 64 filters ReLU
 Conv2D + MaxPool 3×3, 128 filters ReLU
 Flatten
 Dense
 Dense
 Dense
 Dense
 Dense (Output)
1152
 576
 288
 144
 7
ReLU
 ReLU
 ReLU
 ReLU
 Softmax
 21×21×64 → 10×10×64
 8×8×128 → 4×4×128
 2304
 1152
 576
 288
 144
 7
 Baseline Training & Results.- Training was run for 10 epochs, with cross-entropy loss.- Best validation accuracy: 0.5496.- FLOPs measured with fvcore: 0.32751 GFLOPs.- PublicTest accuracy reported after training.
 Baseline Data Augmentation.
 The baseline experiment used a limited augmentation process. Standard transformations included grayscale
 normalization, random cropping, and horizontal flips.
 HybridEffNet and Other Experiments.
 The second architecture builds on transfer learning and modern modules:- HybridEffNet: EfficientNet-B0 backbone (pretrained on ImageNet).- SobelLayer as an input stem to enhance edge information.- Optional CBAM (Convolutional Block Attention Module) for spatial-channel attention.- A new classifier head adapted to 7 classes with dropout regularisation.- Instantiated as: HybridEffNet(num_classes=7, classifier_dropout=0.30, use_cbam=True).
 HybridEffNet Backbone Summary.
 Stage
 Resolution (approx.) Channels Notes
 Stem
 48×48
 Block 1–2 24×24
 Block 3–4 12×12
 32
 16–24
 40–80
 Conv3×3
 Depthwise separable conv
 MBConv, squeeze-and-excite
 1
Stage Resolution(approx.) Channels Notes
 Block5–6 6×6 112–192 DeeperMBConv,skipconnections
 Block7 3×3 320 Finalhigh-level features
 Head 1×1 1280 Globalavgpool+dropout+classifier
 HybridEffNetTraining&Results.-Configuredfor70epochs,earlystoppedat61/70duetoplateau.-Validationaccuracy: 0.6952.-FinalPublicTestaccuracy: 0.7040.-FLOPs: 74.65Mops.-Efficiency: 934.64(Acc%/GFLOPs).
 ExperimentInventory.
 Experiment Model Executed Epochs BestValAcc TestAcc FLOPs Efficiency
 Probe Baseline
 CNN
 Yes–Forwardonly––
Training Baseline
 CNN
 Yes 10 0.5496 0.55 0.3275
Training Hybrid
 EffNet
 Yes 61 0.6952 0.70 0.0747 934.64
 Input-sizesweep.
 Toevaluatethetrade-offbetweenaccuracyandcomputational cost,multiple input sizesweretestedwith
 HybridEffNet:
 InputSize TestAcc. Efficiency(Acc%/GFLOPs)
 96×96 70.40% 934.64
 80×80 69.23% 1150
 104×104 72.26% 627.22
 Interpretation.-Increasingresolutionfrom96×96to104×104improvesaccuracyslightlybutreducesefficiency.-Reducingresolutionto80×80sacrificesasmallamountofaccuracybutboostsefficiency.-96×96isaPareto-optimalchoicebalancingaccuracyandefficiency.
 Reducingcomputationalcost&improvingtheratio.
 ThecompactpretrainedbackbonekeepsFLOPs low,whileaugmentationandcalibratedloss improveaccu
racyatconstantcompute. Inputresolutionscalingprovidesadirectwaytoadjustaccuracy-costtrade-offs.
 Thefinal96×96configurationachieved70.40%PublicTestaccuracyat74,650,482opswithefficiency934.64.
 Limitations.-Facialexpressionsinlow-resolutionimagesremainambiguous.-Confusionbetweenvisuallysimilarexpressions(e.g., fearvssurprise)persists.-Onlytwoarchitectureswereexplored;moreadvancedfamilies(ResNet,ViTs)wereoutofscope.-Furtherablationofaugmentationstrategieswasnotfullyreported.
 Conclusions.
 Withinthe frozenevaluationprotocol, thefinal systemcombinesapretrainedEfficientNetbackbonewith
 richFER-specificaugmentationandcalibratedloss.ThisoutperformsthebaselineCNNsignificantlywhile
 maintainingcontrolledFLOPs. Input-sizetuningallowsflexibleoptimisationalongtheaccuracy–efficiency
 frontier. Thebestmodel,HybridEffNet, achieved70.40%PublicTestaccuracyat74.65Mopswith
 efficiency934.64.
 2
References
 Carrier, P.-L. & Courville, A., 2015. FER-2013: The Facial Expression Recognition 2013 Dataset. In:
 Challenges in representation learning: A report on three machine learning contests. Neural Networks, 64,
 pp. 59–63. [Wolfram Data Repository].
 Tan, M. & Le, Q.V., 2019. EfficientNet: Rethinking Model Scaling for Convolutional Neural Networks. In:
 Proceedings of the 36th International Conference on Machine Learning (ICML). PMLR, 97, pp. 6105–6114.
 Woo, S., Park, J., Lee, J.-Y. & Kweon, I.S., 2018. CBAM: Convolutional Block Attention Module. In:
 European Conference on Computer Vision (ECCV). Lecture Notes in Computer Science, 11211, pp. 3–19.
 Khaireddin, Y. & Chen, Z., 2021. Facial Emotion Recognition: State of the Art Performance on FER2013.
 arXiv preprint arXiv:2105.03588.
 Dosovitskiy, A., Beyer, L., Kolesnikov, A., Weissenborn, D., Zhai, X., Unterthiner, T., Dehghani, M., Min
derer, M., Heigold, G., Gelly, S., Uszkoreit, J. & Houlsby, N., 2021. An Image is Worth 16×16 Words:
 Transformers for Image Recognition at Scale. arXiv preprint arXiv:2010.11929.
 3
