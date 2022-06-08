## Qualitycheck Application

all version info by qualitycheck datasets

| model       | version | epoch | dataSet       | inference dir                        | onnx dir                        |
| ----------- | ------- | --------------- | ------------- | ------------------------------------ | ------------------------------- |
| ppliteseg | v1.0    | e100   | img   | inference_qc            | pp_liteseg.onnx  |
| deeplabv3p | v1.0  | e100   | img | inference_qc | deeplabv3p_qc_1.0.onnx


### Model export and convert

```bash
# train
python train.py        --config configs/qualitycheck/deeplabv3p_resnet50_os8_qualitycheck_512x512_80k.yml       --use_vdl        --save_interval 500        --save_dir output

#val
python val.py    --config configs/qualitycheck/deeplabv3p_resnet50_os8_qualitycheck_512x512_80k.yml        --model_path output_qc/iter_100/model.pdparams

# predict
python predict.py        --config configs/qualitycheck/deeplabv3p_resnet50_os8_qualitycheck_512x512_80k.yml        --model_path output/best_model/model.pdparams        --image_path data/chaotiantest/        --save_dir output_qc/result/chaotiantest/

# export
python export.py  --config configs/qualitycheck/deeplabv3p_resnet50_os8_qualitycheck_512x512_80k.yml  --save_dir output_qc/inference/      --model_path output_qc/iter_100/model.pdparams  --input_shape 1 3 512 512

# convert
paddle2onnx --model_dir output_qc/inference             --model_filename model.pdmodel             --params_filename model.pdiparams             --opset_version 11             --save_file models/deeplabv3p_qc_1.0.onnx
```
