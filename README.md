## Document Classification Service

This project is a document classification service that utilizes the Donut model for classifying different types of documents. The service processes PDF files, converts them into images, and runs inference using a pre-trained model to determine the document type. The results are structured in JSON format and stored in a MinIO object storage.

## Features

- Converts PDF documents into images.
- Runs classification on each image using the Donut model.
- Uploads processed images and results to MinIO.
- Supports batch processing of PDF files.
- Automatically organizes uploaded files into date-based directories.

## Prerequisites

- Python 3.8 or higher
- Docker (if using Docker)
- MinIO server running locally or remotely

## Installation

1. Clone this repository:

   ```bash
   git clone <repository-url>
   cd <repository-directory>
