# ocr-font-format

## Getting Started

Create a new environment
```
conda create --name ocr python=3.8.5
conda activate ocr
```
Then install the requirements. The following code was tested on Python 3.8.5 and pytorch >= 1.10
```
pip install -r requirements.txt
```

## Generating the Data

The images and datasets can be generated by running:
```
python create_data.py
```
The datasets augmented with colored text and blurring are saved in `train2.py`, `val2.py`, and `test2.py`. 
The original datasets created before these changes are saved in `train.py`, `val.py`, and `test.py`.

## Training the Model
The training script is adapted from [huggingface transformers finetuning](https://github.com/huggingface/transformers/blob/27c1b656cca75efa0cc414d3bf4e6aacf24829de/examples/run_lm_finetuning.py). 
However, I ended up not needing the distributed training support, because I only used 1 GPU for training these models.

The model checkpoints will be stored in the `experiments` directory.
### Training ClipClassifier
Begin fine-tuning CLIP on the synthetic dataset by running the following command:
```
python train.py --output_dir experiments/best_clip_model2_2e-5 --save_every_epoch --overwrite_output_dir --num_train_epochs 3 --fp16 --learning_rate 2e-5 --model_name RN50x16 --resize 
```
If `RN50x16` is not listed by `clip.available_models()` for some reason, we can alternatively use the downloaded pretrained CLIP model checkpoint by using the argument `--model_name ./RN50x16`.

The ClipClassifier model checkpoints and training logs will be saved in the output directory `experiments/best_clip_model2_2e-5`. 

### Training SimpleClassifier
Begin training SimpleClassifier by running the following command:
```
python train.py --output_dir experiments/best_simple_model2_2e-5 --save_every_epoch --overwrite_output_dir --num_train_epochs 3 --fp16 --learning_rate 2e-5 --model_name RN50x16 --resize 
```
The SimpleClassifier model checkpoints and training logs will be saved in the output directory `experiments/best_simple_model2_2e-5`. 

## Demo
After training our models, we can see their predictions on the front page of the [TextOCR paper](https://arxiv.org/pdf/2105.05486.pdf) using `font_style_classification_demo.ipynb`.
