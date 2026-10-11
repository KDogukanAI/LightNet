# LightNet

**Low-light small-object detection research by DK AI LAB.**

> Same AI. Same camera. Different lighting.

LightNet investigates how object detectors can preserve useful small-object information under low illumination while remaining practical for edge deployment. The project connects reproducible academic experiments with a field-oriented demonstration using a single RGB camera.

**Status: research planning.** This repository currently documents the proposed direction. Training code, model weights, benchmark results, and a deployment demo are not yet available. Architectural choices are hypotheses to test; state-of-the-art performance and production readiness have not been established.

## Motivation

Small objects occupy few pixels. Under low light, reduced contrast, sensor noise, and motion blur can make those pixels difficult to distinguish from the background. Denoising may remove useful detail, while enhancement may amplify noise.

LightNet asks:

**Can spatial reliability-guided feature fusion improve small-object detection in real low-light scenes at a comparable computational cost?**

The initial proposed application is pedestrian, bicycle, and vehicle detection. Final classes, hardware, and operating conditions will be fixed in the experiment protocol.

## Research objectives

- Establish strong pretrained and low-light fine-tuned baselines.
- Measure detection failures jointly across object size and illumination.
- Investigate how to preserve fine detail without amplifying background noise.
- Evaluate generalization to unseen scenes and cameras.
- Measure accuracy alongside end-to-end latency and resource use.
- Release reproducible experiments and a transparent demonstration, including failure cases.

## Proposed architecture

The first study will focus on single-frame RGB detection built on a reproducible pretrained detector.

| Component | Intended role |
| --- | --- |
| Multi-scale backbone | Extract fine detail and semantic context. |
| High-resolution detection branch, such as stride 4/P2 | Preserve information about small objects. |
| Lightweight reliability estimator | Predict spatial feature usefulness under illumination degradation and noise. |
| Reliability-guided fusion | Balance high-resolution detail with deeper contextual features. |
| Detection heads | Predict object classes and bounding boxes. |

The reliability output is a learned feature map, not a calibrated physical noise or signal-to-noise estimate unless separately supervised and validated.

Paired bright/dark supervision and realistic degradation augmentation are candidate training strategies. Temporal fusion is a possible later extension. Novelty will be assessed against relevant prior work before making a contribution claim.

## Data and evaluation

Candidate datasets:

- [NOD](https://github.com/igor-morawski/NOD): outdoor low-light detection of people, bicycles, and cars.
- [DarkVision](https://arxiv.org/abs/2301.06269): paired low-light image/video perception data for controlled illumination studies.
- [ExDark](https://github.com/cs-chan/Exclusively-Dark-Image-Dataset): diverse low-light images with object annotations.
- Dedicated camera recordings for held-out field evaluation.

Dataset selection depends on annotation quality, small-object coverage, access, and applicable usage terms. Dataset licenses remain separate from this repository's license.

### Experimental controls

1. Preserve official benchmark splits and document class mappings.
2. Keep related scenes, adjacent frames, and exposure variants out of different train/validation/test splits.
3. Define object-size categories and report dimensions at the actual model input resolution.
4. Use real low-light test data; synthetic degradation supplements training.
5. Compare methods under documented training, supervision, resolution, and compute budgets.
6. Select operating thresholds on validation data and freeze them before final testing.
7. Report repeated runs and uncertainty with scene-level dependence in mind.

### Baseline and ablation plan

| Experiment | Purpose |
| --- | --- |
| Pretrained detector | Measure the initial low-light performance gap. |
| Low-light fine-tuning | Quantify improvements from data alone. |
| Degradation augmentation | Test the benefit of simulated low-light conditions. |
| Higher input resolution | Control for retaining more image detail. |
| P2 branch without reliability fusion | Isolate high-resolution architecture effects. |
| Enhancement followed by detection | Compare against image preprocessing. |
| Proposed fusion | Test the central research hypothesis. |
| Constant and brightness-based fusion | Test whether learned reliability adds value. |
| Comparable-compute baseline | Separate architectural benefit from extra computation. |

### Metrics

- AP across IoU thresholds, small-object AP, and recall at a fixed false-positive rate.
- Performance by object size and illumination, with sample counts.
- Normal-light performance and transfer to unseen scenes/cameras.
- End-to-end p50/p95 latency, sustained throughput, memory, and power where measurable.

Success criteria will be fixed after baseline measurement and hardware selection, before final test evaluation. No benchmark results are currently claimed.

## Field demonstration

The planned demo will compare the baseline and proposed model on identical input frames, with synchronized detections, small-object crops, and measured errors on annotated sequences.

Controlled captures will vary illumination and distance independently, recording exposure, gain, resolution, frame rate, and illuminance where available. Separate tests will cover fixed camera settings and the intended operational camera configuration.

Deployment evaluation will include frame age, dropped frames, stream recovery, prolonged operation, and thermal behavior. Each model's latency will be measured independently to avoid contention from running comparisons simultaneously.

The educational demo will show where the method succeeds and where it fails. Field applicability will be limited to the operating conditions actually tested.

## Roadmap

- [ ] Finalize application, camera, training hardware, and deployment target.
- [ ] Complete a focused related-work and novelty review.
- [ ] Audit datasets and freeze the evaluation protocol.
- [ ] Implement reproducible baseline training and evaluation.
- [ ] Analyze failures by object size and lighting.
- [ ] Implement and ablate reliability-guided fusion.
- [ ] Evaluate cross-scene and cross-camera generalization.
- [ ] Profile and validate deployment on the target device.
- [ ] Release the demo, reproducibility materials, and research manuscript.

## Getting started

Implementation is pending. Installation, dataset preparation, training, evaluation, and inference instructions will be added alongside runnable code.

## Starting references

- [Boosting Object Detection with Zero-Shot Day-Night Domain Adaptation — CVPR 2024](https://zpdu.github.io/DAINet_page/)
- [SET: Spectral Enhancement for Tiny Object Detection — CVPR 2025](https://openaccess.thecvf.com/content/CVPR2025/html/Sun_SET_Spectral_Enhancement_for_Tiny_Object_Detection_CVPR_2025_paper.html)
- [Towards RAW Object Detection in Diverse Conditions — CVPR 2025](https://openaccess.thecvf.com/content/CVPR2025/html/Li_Towards_RAW_Object_Detection_in_Diverse_Conditions_CVPR_2025_paper.html)
- [DarkVision: A Benchmark for Low-light Image/Video Perception](https://arxiv.org/abs/2301.06269)

These references guide the investigation; they do not establish novelty for the proposed method.

## License

See [LICENSE](LICENSE) for the repository's GNU General Public License, Version 2 text. Third-party datasets, dependencies, and pretrained weights retain their respective licenses.

---

A **DK AI LAB** research project exploring low-light computer vision, small-object detection, and practical Edge AI.
