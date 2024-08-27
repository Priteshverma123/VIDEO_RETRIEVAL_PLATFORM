# Setting Up MinIO for Video Retrieval Platform

This guide will walk you through the setup process for MinIO on a Windows machine, which is required for the Video Retrieval Platform. Follow the instructions below to download and configure MinIO.

## Download MinIO for Windows

1. **Download MinIO:**

   Visit the [MinIO download page](https://min.io/download?license=agpl&platform=windows) to get the MinIO executable for Windows.

2. **Run the following commands in PowerShell to complete the setup:**

   ```powershell
   # Download the MinIO executable
   Invoke-WebRequest -Uri "https://dl.min.io/server/minio/release/windows-amd64/minio.exe" -OutFile "C:\minio.exe"

   # Set the MinIO root user
   setx MINIO_ROOT_USER admin

   # Set the MinIO root password
   setx MINIO_ROOT_PASSWORD password

   # Start the MinIO server
   C:\minio.exe server C:\Data --console-address ":9001"
   ```

   This will start the MinIO server and make it accessible at `http://127.0.0.1:9000` for the API and `http://127.0.0.1:9001` for the web console.

## Access MinIO

1. **Open your web browser and navigate to the MinIO web console:**

   ```plaintext
   http://127.0.0.1:9001
   ```

2. **Log in with the following credentials:**

   - **Username:** `admin`
   - **Password:** `password`

3. **Create a Bucket:**

   - After logging in, create a new bucket named `cctvfootages`. This bucket will be used by the Video Retrieval Platform to manage video storage.

4. **Upload Video Files:**

   - Navigate to the `cctvfootages` bucket in the MinIO console.
   - Click on the "Upload" button and select the video files from your `Data` folder to upload them to the `cctvfootages` bucket.

## Run Video Ingestion Script

After uploading videos to the MinIO bucket, you need to process them for embedding and indexing in Pinecone. Run the following script:

```bash
python video_ingestion.py
```

This script handles:

- Loading video files from MinIO.
- Extracting frames from the videos.
- Embedding the frames using the CLIP model.
- Storing the resulting embeddings in the Pinecone vector index.

Make sure to run this script every time you add new videos to the MinIO bucket.

## Example Configuration in Your Project

```python
import os
from minio import Minio

# Initialize MinIO client
client = Minio(
    DevelopmentConfig.MINIO_ENDPOINT,
    access_key=DevelopmentConfig.MINIO_ACCESS_KEY,
    secret_key=DevelopmentConfig.MINIO_SECRET_KEY,
    secure=False
)
```

This configuration will allow your project to interact with MinIO for storing and retrieving video files.

## Additional Configuration

1. **Create a .env file:**

   After setting up MinIO, create a `.env` file in your project directory. Add the following lines to configure your environment variables:

   ```plaintext
   MINIO_ENDPOINT=127.0.0.1:9000
   MINIO_ACCESS_KEY=admin
   MINIO_SECRET_KEY=password
   PINECONE_API_KEY=your_pinecone_api_key
   ```

2. **Run the following Python files:**


   - **Retrieve Videos Based on Queries:**

     ```bash
     python main.py
     ```

     This file will process queries and retrieve relevant videos from MinIO.

## Summary

By following these steps, you have set up MinIO on your Windows machine and configured it for use with the Video Retrieval Platform. You have also created a bucket named `cctvfootages` and uploaded your video files, enabling efficient and scalable video retrieval. Additionally, you've set up environment variables for seamless integration and provided Python scripts for connecting to MinIO, embedding video frames, and handling video storage and retrieval.

