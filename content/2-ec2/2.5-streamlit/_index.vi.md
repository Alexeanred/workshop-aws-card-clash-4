---
title : "Cài đặt streamlit application"
date :  "`r Sys.Date()`" 
weight : 5
chapter : false
pre : " <b> 2.5 </b> "
---

### Update hệ thống và cài python
```
sudo yum update -y
sudo yum install python3
sudo yum install pip
sudo yum install python3-venv

``` 
### Cài đặt virtual env
```
python3 -m venv appEnv  # Tạo môi trường ảo tên là appEnv 
```
### Cài đặt thư viện
```
source appEnv/bin/activate  # Kích hoạt môi trường ảo
pip install streamlit requests boto3 mysql-connector-python Pillow sagemaker pandas
pip3 list
```
### Tạo file app.py
```
touch app.py
vi app.py
```

```python
import streamlit as st
import boto3
from sagemaker import Session
from sagemaker.huggingface import HuggingFacePredictor
from sagemaker.serializers import DataSerializer
from PIL import Image
import io
import logging
import os
import mysql.connector
from datetime import datetime

# MySQL database configuration
db_config = {
    'user': 'root',    # replace with your MySQL username
    'password': 'Okbaby123@',  # replace with your MySQL password
    'host': '172.31.44.44',      # replace with your DB host (e.g., 'localhost')
    'database': 'prediction_data'   # replace with your database name
}

# Initialize MySQL connection
def get_db_connection():
    conn = mysql.connector.connect(**db_config)
    return conn

# Set region for boto3 client
boto3.setup_default_session(region_name='us-east-1')

# Initialize SageMaker session
sagemaker_session = Session()

# Configure SageMaker endpoint
ENDPOINT_NAME = "huggingface-pytorch-inference-2024-10-20-14-51-51-933" # change

# Use HuggingFacePredictor to call the SageMaker endpoint
predictor = HuggingFacePredictor(endpoint_name=ENDPOINT_NAME, sagemaker_session=sagemaker_session)

# S3 Configuration
S3_BUCKET = "vinfast-car-images-150903"  # change
s3_client = boto3.client('s3')

LOG_FILE = "test2.log"

# Tạo file log nếu chưa tồn tại
if not os.path.exists(LOG_FILE):
    with open(LOG_FILE, 'w'):
        pass

# Cấu hình logging
logger = logging.getLogger()
logger.setLevel(logging.INFO)

# Xóa bất kỳ handler nào hiện có (tránh xung đột)
if logger.hasHandlers():
    logger.handlers.clear()

# Thêm handler ghi vào file
file_handler = logging.FileHandler(LOG_FILE)
formatter = logging.Formatter('%(asctime)s - %(levelname)s - %(message)s')
file_handler.setFormatter(formatter)
logger.addHandler(file_handler)

# Check if the app has already been started
if 'app_started' not in st.session_state:
    logging.info('Streamlit app started!')
    st.session_state.app_started = True  # Set the flag to True

# This will detect the image format and assign the correct content type
def detect_image_format(file_name):
    if file_name.endswith(".png"):
        return 'image/png'
    elif file_name.endswith(".jpg") or file_name.endswith(".jpeg"):
        return 'image/jpeg'
    else:
        raise ValueError("Unsupported image format.")

def predict_image(image_file):
    # Convert the uploaded file into a bytes stream
    file_name = image_file.name
    img_bytes = io.BytesIO(image_file.read())  # Read the image file into BytesIO object
    if img_bytes.getbuffer().nbytes == 0:
        raise ValueError("No image data found.")  # Check if the BytesIO object is empty
    
    # Detect content type based on file extension
    content_type = detect_image_format(file_name)
    
    # Update predictor's serializer with correct content type
    predictor.serializer = DataSerializer(content_type=content_type)

    # Send the image to SageMaker endpoint for inference
    result = predictor.predict(img_bytes.getvalue())  # Send bytes data for prediction

    # Return the result
    return result

def upload_to_s3(file_obj, bucket_name, object_name):
    try:
        s3_client.upload_fileobj(file_obj, bucket_name, object_name)
        logger.info(f"Successfully uploaded {object_name} to S3 bucket {bucket_name}")
        return f"s3://{bucket_name}/{object_name}"
    except Exception as e:
        logger.error(f"Failed to upload file to S3: {e}")
        raise

def save_to_db(image_url, prediction, upload_time):
    try:
        conn = get_db_connection()
        cursor = conn.cursor()
        
        # Insert record into database
        query = ("INSERT INTO predictions (image_url, prediction, upload_time) "
                 "VALUES (%s, %s, %s)")
        prediction_str = ", ".join([f"{pred['label']}: {round(pred['score'] * 100, 2)}%" for pred in prediction])
        cursor.execute(query, (image_url, prediction_str, upload_time))
        
        conn.commit()
        cursor.close()
        conn.close()
        
        logger.info(f"Successfully saved prediction for {image_url} to database")
    except mysql.connector.Error as err:
        logger.error(f"Error: {err}")
        st.error(f"Error saving to database: {err}")

import pandas as pd

# Function to display predictions nicely
def display_predictions(predictions):
    # Parse the predictions and extract labels and scores
    formatted_predictions = [{"Car Model": pred["label"], "Confidence Score": round(pred["score"] * 100, 2)} for pred in predictions]

    # Create a pandas DataFrame for better formatting
    df = pd.DataFrame(formatted_predictions)
    
    # Display the DataFrame as a table
    st.write("### Classification Results:")
    st.table(df)

# Streamlit file upload interface
st.title('VinFast Car Classifier')
uploaded_file = st.file_uploader("Upload an image of VinFast car", type=["jpg", "png", "jpeg"])

if uploaded_file is not None:
    try:
        # Display uploaded image
        image = Image.open(uploaded_file)
        st.image(image, caption="Uploaded VinFast Car Image.", use_column_width=True)

        # Reset the file stream so it can be used again after displaying the image
        uploaded_file.seek(0)  # Reset the file pointer to the beginning

        # Perform inference and display the result
        st.write("Classifying the image...")
        
        # Predict using the file
        prediction = predict_image(uploaded_file)  # Use uploaded_file directly
        display_predictions(prediction)

        # Now we handle S3 upload and database insertion
        uploaded_file.seek(0)  # Reset the file pointer to the beginning for S3 upload
        img_buffer = io.BytesIO(uploaded_file.read())  # Store the file in a buffer for S3 upload

        # Upload to S3
        img_buffer.seek(0)  # Reset the buffer before upload
        s3_url = upload_to_s3(img_buffer, S3_BUCKET, uploaded_file.name)  # Upload image to S3

        # Save the prediction and S3 URL to the database
        upload_time = datetime.now().strftime('%Y-%m-%d %H:%M:%S')
        save_to_db(s3_url, prediction, upload_time)

    except Exception as e:
        st.error(f"Error during prediction or upload: {e}")
        logger.error(f"Error during prediction or upload: {e}")
```

### Tạo services streamlit
```
sudo vi /etc/systemd/system/streamlit-app.service

[Unit]
Description=Streamlit App
After=network.target

[Service]
User=ec2-user
WorkingDirectory=/home/ec2-user
Environment="PATH=/home/ec2-user/appEnv/bin"
ExecStart=/home/ec2-user/appEnv/bin/streamlit run /home/ec2-user/app.py --server.port 8080
Restart=always

[Install]
WantedBy=multi-user.target

sudo systemctl daemon-reload
sudo systemctl enable streamlit-app.service
sudo systemctl start streamlit-app.service
sudo systemctl status streamlit-app.service
sudo reboot
sudo systemctl status streamlit-app.service
```



