---
title : "Tạo sagemaker endpoint"
date :  "`r Sys.Date()`" 
weight : 2
chapter : false
pre : " <b> 2.2 </b> "
---
### Lấy code để deploy model
* Truy cập vào huggingface, ta sẽ lấy model đã train rồi để dùng.
* [Link model](https://huggingface.co/hungtu/vinfast-car-classifier/tree/main)
![sg](/workshop-aws-card-clash-4/images/2.prerequisite/2.0.png) 
* Vào mục **Deploy**, chọn **Amazon Sagemaker**.
* Copy code để lại để dùng trong notebook.
![sg](/workshop-aws-card-clash-4/images/2.prerequisite/2.2_.png) 
### Chạy notebook và tạo endpoint
* Vào Amazon SageMaker, chọn **Notebooks**:
* Chọn **Create notebook instance**:
![sg](/workshop-aws-card-clash-4/images/2.prerequisite/2.3_.png) 
* **Notebook instance name**: ```vinfast-prediction```.
* Để các setting default như hình.
* Chọn **Create notebook instance**
![sg](/workshop-aws-card-clash-4/images/2.prerequisite/2.4_.png) 
* Đợi vài phút để notebook dc tạo, khi nào Status là InService là ok.
![sg](/workshop-aws-card-clash-4/images/2.prerequisite/2.7_.png) 
* Sau khi instance được tạo xong, Chọn **Open JupyterLab**.
* Chọn File -> New -> Notebook.
* Chọn **kernel: conda_tensorflow2_p310**.

* Chạy code dưới đây:
```python
import sagemaker
import boto3
from sagemaker.huggingface import HuggingFaceModel
from sagemaker.serializers import DataSerializer

s3 = boto3.client('s3')
s3.download_file('vinfast-car-images-150903', 'vinfast.jpg', 'vinfast.jpg')
try:
    role = sagemaker.get_execution_role()
except ValueError:
    iam = boto3.client('iam')
    role = iam.get_role(RoleName='sagemaker_execution_role')['Role']['Arn']

# Hub Model configuration. https://huggingface.co/models
hub = {
    'HF_MODEL_ID':'hungtu/vinfast-car-classifier',
    'HF_TASK':'image-classification'
}

# create Hugging Face Model Class
huggingface_model = HuggingFaceModel(
    transformers_version='4.37.0',
    pytorch_version='2.1.0',
    py_version='py310',
    env=hub,
    role=role, 
)

# deploy model to SageMaker Inference
predictor = huggingface_model.deploy(
    initial_instance_count=1, # number of instances
    instance_type='ml.m5.xlarge' # ec2 instance type
)



predictor.serializer = DataSerializer(content_type='image/x-image')

# Make sure the input file "cats.jpg" exists
with open("vinfast.jpg", "rb") as f:
    data = f.read()
predictor.predict(data)
```
* Trong code ở trên, mình đã thử lấy 1 hình ảnh xe mình đã upload lên S3 bucket để test thử model. Bạn có thể upload 1 hình ảnh lên S3 bucket và test xem model ok không nhé.
![sg](/workshop-aws-card-clash-4/images/2.prerequisite/2.8_.png) 
* Vào phần **Inference**, chọn **endpoint**:
* Ta thấy được endpoint vừa dc tạo qua notebook.
![sg](/workshop-aws-card-clash-4/images/2.prerequisite/2.9_.png) 