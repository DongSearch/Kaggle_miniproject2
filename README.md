# 🛠 Consequence(Summary) 🏆
## Goal : 
reach to best accruacy with data sets having large distribution per each class
## Condition
- training data : 70k,3x128x128, class:21(balanced dataset)
- no pretrained model
- no change number of seed(0)


## Used Algorithm
- model : ConvNext-Tiny
- optimizer : Warmup LRscheduling(Linear) + main LRscheduling(CosineAnnealing) + AdamW 
- Criterion : SoftTargetCrossEntorpy(for Mixup/Cutmix) + CrossEntropy + label-smoothing
- Data-Preprocessing : RandAugment,HorizontalFlip,Mixup/Cutmix
- Technique : TTA + EMA + Focal Loss + Autocast

## Result(81% unofficially)

<img width="1590" height="490" alt="image" src="https://github.com/user-attachments/assets/b86188ab-f71d-42ad-8358-28f41e5f09f7" />
<img width="1935" height="790" alt="image" src="https://github.com/user-attachments/assets/0a12b2d9-3e86-400e-856e-f6f8f79c2374" />



# My Journey

## 🛠 Algorithms Used(& Trial):
### optimization
- AdamW
- Earlystopping
- warmup scheduler(Linear) 
- main scheudler(ConsineAnnealingLR)
- Focal Loss
### config
- epoch : 10 -> 50 -> 100->120->200->150
- batchsize : 128->64->256->512->256
### DataAugumentation
- RandomHorizontalFlip
- RandomAugment
- RandomErasing 
- MixUp / CutMix
### TEST
- TTA 2 data augumentation
### Model(ConvNext-tiny)
- ConvNext-tiny(motived by transformer)
- label smoothing 
- Stochastic Depth

## 📅 Key milestone(summary)
- **Feb 03** use model used in first competition and analysis the problems
- **Feb 04** change model to resnet 34
- **Feb 05** change model to ConvNext-Tiny
- **Feb 06** reduce data augumentation and change head of network(more deeply)
- **Feb 07** increase image size(128->176) and adjust data augumentation(stable learning)
- **Feb 08** add focal loss and resize image




## 🧠 Diary(Detail)
### Trial(Feb 03)
### result
- running basic code(accuracy 52%)
<img width="1590" height="490" alt="1" src="https://github.com/user-attachments/assets/0a6dc4b3-28c7-437e-b2f2-5a791abb6251" />

<img width="1935" height="790" alt="1-1" src="https://github.com/user-attachments/assets/022bd32b-3890-4b5e-a766-c28856841dd9" />

### problem
- low accuracy in both training and validation
### Analysis
- too much regularization(no overfit, no underfit)


### Trial(Feb 04)
- change model to resnet50
### result
- running basic code(accuracy 45%)

### problem
- resnet 50 looks like being not enough to capture detail in data
- unstable training(too much variation especially in val_loss
### Analysis
- too much regularization(no overfit, no underfit)
- not sufficient model to extract feature


### Trial(Feb 05)
- change model to ConvNext tiny
### result
- running basic code(accuracy 68%)
<img width="1589" height="490" alt="image" src="https://github.com/user-attachments/assets/19a2a1cd-4e29-462a-aa2e-e7b738c022eb" />


<img width="1935" height="790" alt="image" src="https://github.com/user-attachments/assets/4f61a386-1043-42cc-b8c5-6de32954277d" />


### problem
- it has stable training, but it is still under 70%, which means it needs more technique or fine-tuning the model!
- underfitting from too much regularization
### Analysis
- it has much stable training and validation loss compared to resnet
- too much weight decay
- too much Data augmentation (mixup + cutmix + randaugement), start with less aug, and gradually augment
- scaling resoultion(?)


### Trial(Feb 06)
- reduce augumentation and change to head in model
### result
- running basic code(accuracy 60%)



### problem
- overfitting
### Analysis
- increase size(after passing the whole layers).. it is just 4*4 images
- adjust augumentation(not overfitting, not underfitting)

### Trial(Feb 07)
- increase image size and increas reduce drop rate
### result
- running basic code(accuracy 80%)
<img width="1590" height="490" alt="image" src="https://github.com/user-attachments/assets/587cea70-a76c-48a8-8c71-cc0bed7bf804" />

<img width="1935" height="790" alt="image" src="https://github.com/user-attachments/assets/c7061452-d0f9-4453-aeed-b854fac61b1c" />


### problem
- overall it looks training well(without any overfitting or undertraining phenomenon)
- need to find way to squeeze accuracy
### Analysis
- increasing size looks so very effective(increase almost 10% accuracy.. but.. training time increase explosively as well)
- increase a little bit more image size and change GPU to A100(?) -> 192
- label_smoothing -> 0.05
-  Fokal loss


### Trial(Feb 08)
- increase image size and increas reduce drop rate(192)
- Foakl loss (50% -> 61%)
### result
- running basic code(accuracy 80%)
<img width="1590" height="490" alt="image" src="https://github.com/user-attachments/assets/b86188ab-f71d-42ad-8358-28f41e5f09f7" />
<img width="1935" height="790" alt="image" src="https://github.com/user-attachments/assets/0a12b2d9-3e86-400e-856e-f6f8f79c2374" />


### problem
- certain classes still have less accuracy compared to others(and wrong estimations spread evenly over whole classes, which is hard to capture which features give confusing to model)
- increasing image size doesn't give any performance improvement any more
### Analysis
- higher gamma on Fokal loss 
- increase a little bit more image size and change GPU to A100(?) -> 192
- label_smoothing -> 0.05
-  Fokal loss (gamma is 2)




