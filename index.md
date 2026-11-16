# Challenges in Deep Learning-Based Small Organ Segmentation: A Benchmarking Perspective for Medical Research with Limited Datasets

> Markdown version of the project page at https://aiml-sdu.github.io/small-organ-segmentation-page/. Interactive figures are on the web page. Every number here also appears there and comes from the paper or its stored results.

[Research](https://aiml-sdu.github.io) / Benchmark

When a lab has nine annotated images, how much of a model leaderboard can it trust? We tuned ten segmentation networks on small histology sections of mouse arteries and asked which parts of the resulting order survive intervals, folds and a second dataset. The answer is one firm first place and noise below it.

Phongsakon Mark Konrad and seven coauthors · Biomedical Signal Processing and Control, August 2026 · [Journal](https://www.sciencedirect.com/science/article/pii/S1746809426017805) · [PDF](https://arxiv.org/pdf/2509.05892) · [Markdown](index.md) · Cite

*Figure.* IllustrationAn artery section under the microscope. One predicted outline is sharp and the others blur into each other. The drawing carries no measured values. The measured board is Figure 1.

## In short

- We tuned ten segmentation models with 100 Bayesian trials each and scored them on nine histology images of injured mouse carotid arteries.

- MedSAM leads under leave-one-out validation with a macro Dice of 0.694, and its bootstrap interval [0.651, 0.734] clears every other model.

- Below first place the intervals chain into each other. SegFormer at [0.517, 0.633] overlaps SAM at [0.466, 0.527], and both overlap models further down.

- During tuning UNet had the best validation Dice, 0.630 against 0.569 for MedSAM, and the order reversed on held-out data.

- Trained on a second dataset of 153 images, the board changes. At nine training images DeepLabV3+ (0.843) and UNet (0.840) lead, and ranks settle only from 50 images on.

## Abstract

**First,** we benchmark ten architectures on a four-class task with nine images. Classical networks, modern CNNs, one vision transformer and three foundation models each get 100 trials of Bayesian hyperparameter search, 1000 runs in all. MedSAM reaches 0.694 macro Dice under leave-one-out validation and 0.643 under 3-fold validation.

**Second,** we ask how much of that order is real. Bootstrap intervals separate MedSAM from the rest, but models ranked 2nd to 5th overlap in a chain. Under leave-one-out validation UNet-ResNet34 and DeepLabV3+ swing by up to 7 rank positions between folds, and the 3-fold Nemenyi test (critical difference 7.82) cannot tell the top models apart.

**Finally,** we test the order on an independent dataset of 153 images. Trained on it at nine images, DeepLabV3+ and UNet lead and the foundation models fall to 3rd to 5th. Across the sizes 9, 25, 50, 100 and 150, rankings stay volatile up to 25 images and stabilise from 50.

What would have refuted us? Intervals that kept all ten models apart, ranks that held from fold to fold, or a second dataset that repeated the first board. We tested all three. The first held only for MedSAM, and the other two failed.

Figure 1 shows the measured board. Each button is one of the validation protocols in the paper, and the frosted label names the protocol in use.

Turn the lens. The same ten models are measured under each protocol, and the board reorders.

*Figure.* **Figure 1. One board, many lenses.** Pick a protocol and the ten models reorder. Values are the paper's reported tables1, and whiskers show the bootstrap 95% confidence interval where the paper reports one2.

## Terms we use

Macro Dice

The overlap between predicted and expert masks, from 0 to 1, averaged over lumen, neointima and media with the background left out.

Leave-one-out validation

Train on eight images and test on the ninth, nine times over. Nearly unbiased, with high variance.

3-fold validation

Split the nine images into three groups and test on each in turn. Lower variance, more bias.

Bootstrap interval

A 95% range for a model's mean score, from resampling its fold scores with replacement 10,000 times.

Cohen's d

The gap between two models' mean scores in units of their spread. Values above 0.8 count as large.

Second dataset

153 images from a separate cohort of mice, photographed under other conditions. We use it for distribution shift and for training at larger sizes.

HD95

The 95th percentile distance in pixels between predicted and true tissue boundaries. Lower is better.

## Only first place survives the intervals

A leaderboard prints one number per model. On nine images each number sits on top of a wide spread, and the spread decides what the order means.

We resampled every model's fold scores 10,000 times. Under leave-one-out validation the interval for MedSAM, [0.651, 0.734], sits clear of every other model. That part of the board is real. Below it the intervals link up. SegFormer overlaps SAM and MedSAM+UNet, and those two overlap UNet at [0.285, 0.470]. So the order from 2nd to 5th cannot be read with confidence.

Pick a model below to see which others its interval touches. Switch to 3-fold validation and the picture shifts again.

*Figure.* **Bootstrap 95% intervals for macro Dice.** Dots are means and bars the intervals, as reported in the paper. Select a model to mark every model whose interval overlaps it.

Effect sizes tell the same story from another side. Gaps between families are large. MedSAM against FCN gives d = 8.54. Gaps inside a family are small, UNet against UNet-ResNet34 gives d = 0.13. Hover the matrix to read any pair.

*Figure.* **Pairwise Cohen's d, leave-one-out macro Dice.** Each cell is the row model against the column model. Darker means a larger gap. Outlined cells pass the 0.8 threshold for a large effect.

## Ranks swing from fold to fold

A mean rank hides how a model got there. We ranked all ten models on every fold and kept the mean, the spread and the range.

MedSAM ranks first on all nine leave-one-out folds and all three 3-fold folds. Nobody else is that steady. UNet-ResNet34 has a rank standard deviation of 2.44 and a range of 7 positions under leave-one-out validation. Under 3-fold validation it barely moves, with a standard deviation of 0.58, and its mean rank drops from 5.78 to 7.67. The protocol changes both where a model lands and how sure we can be of it.

Ten models scored on nine images give the Friedman test enough to find that some ranks differ, with p < 0.001 under leave-one-out validation and p = 0.003 under 3-fold validation. Three folds are too few to say which, and the critical difference of 7.82 joins all the top models.

*Figure.* **Mean rank across folds, plus or minus one standard deviation.** Rank 1 is best. The number on the right is the rank range, the gap between a model's best and worst fold. Values from the paper's rank instability table.

## A second dataset writes a different board

If the order on nine images reflected the architectures, a second dataset should repeat it. We collected 153 images from a separate cohort, held three out as a fixed test set, and trained every model on nested subsets of 9, 25, 50, 100 and 150 images with the hyperparameters tuned on the first dataset.

At nine images DeepLabV3+ leads with 0.843 and UNet follows with 0.840. On the first dataset these two placed 7th and 5th. The three foundation models, which held the top of the first board, rank 3rd to 5th. SegFormer starts 7th at 0.717 and ends 1st at 0.906 with 150 images. Ranks stay volatile up to 25 images and settle from 50.

Drag the slider to change the training size and watch the order.

*Figure.* **Trained on the second dataset.** Mean macro Dice on the three held-out images for each training size. The ranked list on the right follows the slider. Hover a line to isolate a model.

We also ran the models tuned on the nine images directly on all 153 new images, under distribution shift. Here the order splits by metric. SAM leads on Dice with 0.546. SegFormer leads on boundary error, with an HD95 of 27.2 pixels. FCN and SegNet fail, with boundary errors above 110 pixels.

*Figure.* **Models from the nine images, scored on the second dataset.** Up is better Dice and left is a smaller boundary error. The two leaders differ.

## Small choices move scores as much as architecture

Data splits are one source of variance. We ran more than 190 ablations to find the others, covering augmentation, input resolution, random seeds and hyperparameters.

Augmentation alone can matter more than the network. Lightweight U-Net ranged from 0.04 to 0.44 Dice across presets. UNet rose from 0.41 without augmentation to 0.66 with strong geometric transforms. MedSAM barely noticed and stayed between 0.67 and 0.70 across all 10 presets. Five random seeds moved scores by a standard deviation of 0.03 to 0.15 Dice, least for the foundation models.

Size buys accuracy at a price. MedSAM has 95.8 million parameters and leads on the nine images. SegFormer has 3.7 million and comes second, at 9.3 ms per image against 121.8 ms. Hover a point to compare.

*Figure.* **Accuracy against size.** Leave-one-out macro Dice against parameters in millions, on a log scale, as reported in the paper's efficiency table.

Our recommendation follows from these four chapters. With nine images, report bootstrap intervals and results under more than one protocol. Expect stable rankings only from 50 to 100 samples, and test on data from another source before choosing a model.

## Scope

### It shows

- On nine images of one tissue, ten tuned models give one statistically separable leader and an unreliable order below it.

- Rankings change with the validation protocol, with the dataset and with the training size.

- Augmentation and seeds add variance on the scale of the gaps between architectures.

### It does not show

- That MedSAM or any other model is best for histology in general.

- Effects across random initialisations for the main runs, which used one seed.

- Second-dataset rankings free of tuning effects, since those runs reused hyperparameters from the first dataset.

- A measured inter-annotator agreement. Two experts annotated by a shared protocol, without an agreement score.

Next we want the same test on other organs and stains, and on more seeds, to see where the threshold of 50 images moves.

## Footnotes

- Every plotted value is copied from the tables of the published paper. Nothing on this page is simulated. ↩

- Whiskers appear only where the paper reports an interval. The other lenses show means alone. ↩

## Cite this article

```bibtex
@article{konrad2026smallorgan,
  title   = {Challenges in Deep Learning-Based Small Organ Segmentation:
             A Benchmarking Perspective for Medical Research with Limited Datasets},
  author  = {Konrad, Phongsakon Mark and Popa, Andrei-Alexandru and
             Sabzehmeidani, Yaser and Zhong, Liang and Tripathy, Madhulika and
             Constantinescu, Andrei and Liehn, Elisa A. and Ayvaz, Serkan},
  journal = {Biomedical Signal Processing and Control},
  volume  = {128},
  pages   = {111226},
  year    = {2026},
  doi     = {10.1016/j.bspc.2026.111226}
}
```

Plotted values come from the published paper's tables.

A project of the [Applied and Interpretable Machine Learning Research Group](https://aiml-sdu.github.io) at the University of Southern Denmark.

Authors

Phongsakon Mark Konrad, Andrei-Alexandru Popa, Yaser Sabzehmeidani, Liang Zhong, Madhulika Tripathy, Andrei Constantinescu, Elisa A. Liehn, Serkan Ayvaz

Affiliations

University of Southern Denmark · Duke-NUS · National Heart Center Singapore · Carol Davila University of Medicine and Pharmacy Bucharest

Published

August 2026

Journal

[Biomedical Signal Processing and Control](https://www.sciencedirect.com/science/article/pii/S1746809426017805)

Full paper

[PDF](https://arxiv.org/pdf/2509.05892) · [arXiv](https://arxiv.org/abs/2509.05892) · [Markdown](index.md) · Cite
