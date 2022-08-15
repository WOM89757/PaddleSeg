## Qualitycheck Application

### Performance

#### Ade20k

| Model | Backbone | Resolution | Training Iters | mIoU | Links | FTP Location |
|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
| DeepLabV3+ | R-50-D8  | 512x512   |   80000 | 42.72% | [model](https://download.openmmlab.com/mmsegmentation/v0.5/deeplabv3plus/deeplabv3plus_r50-d8_512x512_80k_ade20k/deeplabv3plus_r50-d8_512x512_80k_ade20k_20200614_185028-bf1400d8.pth) &#124; [log](https://download.openmmlab.com/mmsegmentation/v0.5/deeplabv3plus/deeplabv3plus_r50-d8_512x512_80k_ade20k/deeplabv3plus_r50-d8_512x512_80k_ade20k_20200614_185028.log.json)         | /wangmao/models/segmentation/ade20k/mmcv/deeplabv3plus_r50-d8_512x512_80k_ade20k.onnx |
|DeepLabV3P|ResNet50_OS8|512x512|40000|41.87%|[model](ftp://192.168.2.23:9021:/wangmao/models/segmentation/ade20k/paddle/deeplabv3p_ade20k_40k.pdparams) &#124; [log](/wangmao/models/segmentation/ade20k/paddle/deeplabv3p_ade20k_80k.log.tgz) | /wangmao/models/segmentation/ade20k/paddle/deeplabv3p_ade20k_40k.pdparams |
|DeepLabV3P|ResNet50_OS8|512x512|80000|43.33%|[model](ftp://192.168.2.23:9021:/wangmao/models/segmentation/ade20k/paddle/deeplabv3p_ade20k_80k.pdparams) &#124; [log](/wangmao/models/segmentation/ade20k/paddle/deeplabv3p_ade20k_80k.log.tgz) | /wangmao/models/segmentation/ade20k/paddle/deeplabv3p_ade20k_80k.pdparams |

#### ADEChallengeData2016_Sky_Road_Tree

| Model | Backbone | Resolution | Training Iters | mIoU | Acc | Links | FTP Location | 
|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
|DeepLabV3P|ResNet50_OS8|512x512|30000|17.92%|82.56%|[model](ftp://192.168.2.23:9021:/wangmao/models/segmentation/ade20k/paddle/deeplabv3p_ade20k_80k.pdparams) &#124; [log](/wangmao/models/segmentation/ade20k/paddle/deeplabv3p_ade20k_80k.log.tgz) | /wangmao/models/segmentation/ade20k/paddle/deeplabv3p_ade20k_80k.pdparams |
|DeepLabV3P|ResNet50_OS8|512x512|2000|96.45%|98.18%|[config](./deeplabv3p_resnet50_ade6k_sky_road_tree_512x512_30k-qc_v1.1.yml) &#124;[model](ftp://192.168.2.23:9021:/wangmao/models/segmentation/ade20k/paddle/deeplabv3p_ade6k_80k.pdparams) &#124; [log](/wangmao/models/segmentation/ade20k/paddle/deeplabv3p_ade20k_80k.log.tgz) | /wangmao/models/segmentation/ade20k/paddle/deeplabv3p_ade20k_80k.pdparams |
|DeepLabV3P|ResNet50_OS8|512x512|1000|97.14%|98.51%|[config](./deeplabv3p_resnet50_ade20k_sky_road_tree_512x512_30k-qc_v1.1.yml) &#124;[model](ftp://192.168.2.23:9021:/wangmao/models/segmentation/ade20k/paddle/deeplabv3p_ade6k_80k.pdparams) &#124; [log](/wangmao/models/segmentation/ade20k/paddle/deeplabv3p_ade20k_80k.log.tgz) | /wangmao/models/segmentation/ade20k/paddle/deeplabv3p_ade20k_80k.pdparams |


mIoU: 0.9602 Acc: 0.9798 

mIoU: 0.9678 Acc: 0.9835

### All Version Info


| Model       | Version | Epoch | DataSet       | FTP Onnx Location                        |
| ----------- | ------- | --------------- | ------------- | ------------------------------------ |
| deeplabv3+ | v0.1  | 64   | ADEChallengeData2016 | /wangmao/project/qualitycheck/deeplabv3plus.onnx |
| deeplabv3p | v1.0  | 64   | ADEChallengeData2016 | /wangmao/project/qualitycheck/deeplabv3p_qc_1.0.onnx |
| deeplabv3p | v1.1  | 32   | ADEChallengeData2016_Sky_Road_Tree | /wangmao/project/qualitycheck/deeplabv3p_qc_1.1.onnx    |


## Infer Test

| model       | version | infer num       | sum infer time(ms)       |   infer time(ms) | fps|              |
| ----------- | ------- |  ------------- | ------------------------------------ | ----|--- | -----|
| deeplabv3+ | v0.1  |  100 | 23007.2109         | 230.07  | 4.35 |



### Datasets Info


| Name       | FTP Location   |
| ----------- | ------- |
| ADEChallengeData2016 |  /wangmao/datasets/ADEChallengeData2016.tgz |
| ADEChallengeData2016_Sky_Road_Tree | /wangmao/datasets/ADEChallengeData2016_Sky_Road_Tree.tgz |


### Model export and convert

```bash
# Custom data convert
python tools/split_dataset_list.py ./data/skyimg1.0 ./ label --split 0.6 0.2 0.2 --format jpg png --label_class 'sky' 'tree' 'building'

# train
python train.py  --config ./configs/qualitycheck/deeplabv3p_resnet50_ade20k_512x512_80k-qc_v1.0.yml  --save_interval 6000  --save_dir ./output_ade20k/  --do_eval   --use_vdl  --resume_model output_ade20k/iter_40000/ --num_workers 10

#val
python val.py    --config configs/qualitycheck/deeplabv3p_resnet50_ade20k_512x512_80k-qc_v1.0.yml        --model_path output_ade20k/best_model/model.pdparams

# predict
python predict.py        --config configs/qualitycheck/deeplabv3p_resnet50_ade20k_512x512_80k-qc_v1.0.yml        --model_path output_ade20k/best_model/model.pdparams        --image_path data/ade20ktest/        --save_dir output_ade20k/result/chaotiantest/

# export
python export.py  --config configs/qualitycheck/deeplabv3p_resnet50_ade20k_512x512_80k-qc_v1.0.yml  --save_dir output_ade20k/inference/      --model_path output_ade20k/best_model/model.pdparams  --input_shape 1 3 512 512

# convert
paddle2onnx --model_dir output_ade20k/inference             --model_filename model.pdmodel             --params_filename model.pdiparams             --opset_version 11             --save_file models/ade20k/onnx/deeplabv3p_ade20k_80k.onnx

```
