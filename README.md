# Regularized Self-Labeling

This repository provides the code for the paper "Improved Regularization and Robustness for Fine-tuning in Neural Networks" accepted in NeurIPS 2021.

We propose an algorithm that incorporates regularized fine-tuning and self-training for improved regularization and robustness.  In the algorithm:

- First, we encode ***layer-wise distance constraints*** to regularize the model weights at different levels. Compared to (vanilla) fine-tuning, our algorithm reduces the gap between the training and test accuracy, thus alleviating over-fitting.
- Second, we add a ***self-labeling mechanism*** that corrects and down weights "noisy labels" based on the neural network's predictions.

![](./figures/main_figure.pdf)

### Requirements

- Python >= 3.6
- PyTorch >= 1.7
- Optuna >= 2.5
- Numpy

### Usage

Our algorithm are based on layer-wise distance constraint and self lable correction and removal. Our algorithm has achieved the significant improvement upon the previous results.

Test accuracy on MIT-Indoor dataset with independent label noise: 

|   Method    |     Noise = 20%      |     Noise = 40%      |     Noise = 60%      |     Noise = 80%      |
| :---------: | :------------------: | :------------------: | :------------------: | :------------------: |
|  **Ours**   | **75.21 $\pm$ 0.46** | **68.13 $\pm$ 0.16** | **57.59 $\pm$ 0.55** | **34.08 $\pm$ 0.79** |
| Fine-tuning |   65.02 $\pm$ 0.39   |   57.49 $\pm$ 0.39   |   44.60 $\pm$ 0.95   |   27.09 $\pm$ 0.19   |

Run following code to replicate above results:

```Python
python train_label_noise.py --config configs/config_constraint_indoor.json --model ResNet18 \
    --reg_method constraint --reg_norm frob \
    --reg_extractor 7.80246991703043 --reg_predictor 14.077402847906 \
    --noise_rate 0.2 --train_correct_label --reweight_epoch 5 --reweight_temp 2.0 --correct_epoch 10 --correct_thres 0.9 

python train_label_noise.py --config configs/config_constraint_indoor.json --model ResNet18 \
    --reg_method constraint --reg_norm frob \
    --reg_extractor 8.47139398080791 --reg_predictor 19.0191127114923 \
    --noise_rate 0.4 --train_correct_label --reweight_epoch 5 --reweight_temp 2.0 --correct_epoch 10 --correct_thres 0.9 

python train_label_noise.py --config configs/config_constraint_indoor.json --model ResNet18 \
    --reg_method constraint --reg_norm frob \
    --reg_extractor 10.7576018531961 --reg_predictor 19.8157649727473 \
    --noise_rate 0.6 --train_correct_label --reweight_epoch 5 --reweight_temp 2.0 --correct_epoch 10 --correct_thres 0.9 
    
python train_label_noise.py --config configs/config_constraint_indoor.json --model ResNet18 \
    --reg_method constraint --reg_norm frob \
    --reg_extractor 9.2031662757248 --reg_predictor 6.41568500472423 \
    --noise_rate 0.8 --train_correct_label --reweight_epoch 5 --reweight_temp 1.5 --correct_epoch 10 --correct_thres 0.9 
```

### Data Preparation

We use seven image datasets in our paper. We list the link for downloading these datasets and describe how to prepare data to run our code below.

- [Aircrafts](https://www.robots.ox.ac.uk/~vgg/data/fgvc-aircraft/): download and extract into `./data/aircrafts`
  - remove the class `257.clutter` out of the data directory
- [CUB-200-2011](http://www.vision.caltech.edu/visipedia/CUB-200-2011.html): download and extract into `./data/CUB_200_2011/`
- [Caltech-256](http://www.vision.caltech.edu/Image_Datasets/Caltech256/): download and extract into `./data/caltech256/`
- [Stanford-Cars](https://ai.stanford.edu/~jkrause/cars/car_dataset.html): download and extract into `./data/StanfordCars/`
- [Stanford-Dogs](http://vision.stanford.edu/aditya86/ImageNetDogs/): download and extract into `./data/StanfordDogs/`
- [Flowers](https://www.robots.ox.ac.uk/~vgg/data/flowers/102/): download and extract into `./data/flowers/`
- [MIT-Indoor](http://web.mit.edu/torralba/www/indoor.html): download and extract into `./data/Indoor/`

Our code automatically handles the split of the datasets. 