---
title : "Create sagemaker endpoint"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 2.2 </b> "
---
### Get code to deploy model
* Access huggingface, we will get the trained model to use.
* [Link model](https://huggingface.co/hungtu/vinfast-car-classifier/tree/main)
![sg](/workshop-aws-card-clash-4/images/2.prerequisite/2.0.png)
* Go to **Deploy**, select **Amazon Sagemaker**.
* Copy the code to use in the notebook.
![sg](/workshop-aws-card-clash-4/images/2.prerequisite/2.2_.png)
### Run notebook and create endpoint
* Go to Amazon SageMaker, select **Notebooks**:
* Select **Create notebook instance**:
![sg](/workshop-aws-card-clash-4/images/2.prerequisite/2.3_.png)
* **Notebook instance name**: ```vinfast-prediction```.
* Leave the default settings as shown.
* Select **Create notebook instance**
![sg](/workshop-aws-card-clash-4/images/2.prerequisite/2.4_.png)
* Wait a few minutes for the notebook to be created, when the Status is InService, it is ok.
![sg](/workshop-aws-card-clash-4/images/2.prerequisite/2.7_.png)
* After the instance is created, Select **Open JupyterLab**.
* Select File -> New -> Notebook.
* Select **kernel: conda_tensorflow2_p310**.

* Run the code below:
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
* In the code above, I tried to get an image of the car I uploaded to the S3 bucket to test the model. You can upload an image to the S3 bucket and test if the model is ok.
![sg](/workshop-aws-card-clash-4/images/2.prerequisite/2.8_.png)
* Go to **Inference**, select **endpoint**:
* We can see the endpoint just created via the notebook.
![sg](/workshop-aws-card-clash-4/images/2.prerequisite/2.9_.png)