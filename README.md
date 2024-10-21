# Document Classification Service

This project is a document classification service that utilizes the Donut model for classifying different types of documents. The service processes PDF files, converts them into images, and runs inference using a pre-trained model to determine the document type. The results are structured in JSON format and stored in a MinIO object storage.

## Features

- Download pdf from minio object storage
- Converts PDF documents into images.
- Runs classification on each image using the Donut model.
- Uploads processed images to MinIO.
- Automatically organizes uploaded files into date-based directories.
- Returns structured data in JSON format for easy integration with extraction service.
## File Structure

```
project-root/ 
├── resources/
│   ├── document_splitter.py
│   ├── model_handler.py
│   └── ... (other resource files)
├── output_images/           # Directory for storing output images
├── Stored_Images/           # Directory for storing processed files
├── .env.classification      # Environment variables
├── requirements.txt         # Python dependencies
└── README.md
```
## Prerequisites

- Python 3.8 or higher      
- Docker
- MinIO server running locally or remotely

## Installation

1. Clone this repository:

   ```bash
   git clone <repository-url>
   cd <repository-directory>
   ```
2. Install required packages:    
   ```bash
   pip install -r requirements.txt
   ```
3. Set up MinIO server and create the necessary buckets and objects name:
   ```bash
   MINIO_EXPORTBILL_PDF_BUCKET = uploads
   MINIO_EXPORTBILL_IMAGES_BUCKET = rpasplittedimage
   ```
## API Endpoints

The endpoint is - classify-and-split
### Request

- **Method**: POST
- **URL**: `classify-and-split`
- **Body**:
  - **Key** `JSON`
  - **Value** `JSON`
      ```json
      {
      "unique_number": "unique_identifier",
      "file_path": "path_to_pdf_file"
      }
Example request body:
```json
{ 
 "unique_number" : "ae4c455aa9086a5316c0321ceb46c27b",
 "file_path" : "Stored_Images/UploadedFilePdf/ae4c455aa9086a5316c0321ceb46c27b.pdf"
}
```  

### Response

The response will contain the page number of the requested pdf, the classified document type of each page of the pdf and the unique id of the pdf.
example of a response output is :

```json
[
       "doc_type": "COMMERCIAL_INVOICE",
        "pages": [
            {
                "page": "3",
                "url": "391a6aeb-5fe5-44b0-8a20-887d1d205586_page_14.jpg"
            },
            {
                "page": "4",
                "url": "391a6aeb-5fe5-44b0-8a20-887d1d205586_page_13.jpg"
            },
        ],
        "unique_number": "391a6aeb-5fe5-44b0-8a20-887d1d205586"
    },
    {
        "doc_type": "CUSTOMER_FORWARDING",
        "pages": [
            {
                "page": "2",
                "url": "391a6aeb-5fe5-44b0-8a20-887d1d205586_page_2.jpg"
            }
        ],
        "unique_number": "391a6aeb-5fe5-44b0-8a20-887d1d205586"
    }
]
``` 
## Usage

1. send a post request to classify-and-split endpoint with the required json body and format with unique id and the filepath.
2. the service will download required pdf from the MinIO
3. the pdf then will get splitted into images
4. the splitted imaged from the pdf will be sent to get predictiontion from the DONUT finetune weight. DONUT will classify the images among 12 classes which are : AIRWAY_BILL, BILL_OF_EXCHANGE, BILL_OF_ENTRY_EXPORT,BILL_OF_LADING, COMMERCIAL_INVOICE, CERTIFICATE_OF_ORIGIN, EXP_FORM,FORWARDING_CARGO_RECEIPT, CUSTOMER_FORWARDING, OTHERS, PACKING_LIST, TRUCK_RECEIPT.
5. the classified result will be structured into the specific response format and returned as json
6. the response json file will be used for the informatione extraction service.




