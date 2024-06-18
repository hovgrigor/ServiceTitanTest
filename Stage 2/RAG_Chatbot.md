# Overview
Firstly we need to extract the data from the pdf files and organize them into chunks. Next we need to use an embedder to vecotize the chunks and feed them into the retriever. Afterwards we chose an LLM model and feed the model the user query and based on that query retrieve information from our retriever and feed it as a context to the LLM.

# Step 1) PDF processor

Firstly we need to extract the text from the pdf file. We will do it using the pymupdf(https://github.com/pymupdf/PyMuPDF) library to extract the contents of the pdf. 

### Pros

•	Fast and easy extraction.

### Cons

•	The text on the images will not be extracted.

•	The text might need to be brought to some standard form before 
proceeding.

# Step 2) Embedder

Next to need to vectorize the data we will do it using the all-MiniLM-L6-v2 (https://huggingface.co/sentence-transformers/all-MiniLM-L6-v2) since for this use case we do not need large vectors.

### Pros

•	Comparatively small vector size with 384 dimensions.

•	Captures the semantic information.

### Cons

•	Small context window.

# Step 3) Vector DB

Next we need to store them we will do it using Milvus (https://github.com/milvus-io/milvus).

### Pros

•	Open source and feature rich.

•	Fast search, integration with other tools, active community support.

### Cons

•	No direct customer support for questions and issues.

# Step 4) Retriever Model

For our retriever model we will use the Denser Retriever(https://github.com/denser-org/denser-retriever) since it works with Milvus.

### Pros

•	Open source and feature rich.

•	Integration with our chosen tools.

### Cons

•	Very new, a lot of bugs might be present.

# Step 5) LLM Response Generation

For the LLM we will use Meta-s llama-3-8B(https://huggingface.co/meta-llama/Meta-Llama-3-8B)

### Pros

•	Large context window.

•	State of the art model, open source.

### Cons

•   Overkill for our use-case.


# Challenges

•	pymupdf does not extract text from images another tool like like pytesseract needs to be used.

•	The text needs to be organized into chunks of size no more than 256 words before feeding it into the embedder since it requires for the word count to be less than 256 words. Some form of standardization needs to be coded so that the sentences have comparatively the same size to be further divided into chunks.

# Questions

### Will fail to answer fully

•	What is the best way to fix the washing mashing whose motor does not work.\
(Not present in the manual, Missing context)

•	Which is the best brand between X brand and Y brand.\
(Overly generic question)

•	Should i use a specific type of wiring to fix the motor which makes too much noise.\
(Too specific with not enough details)

•	In documents A, B, C where is the image with description of toaster X.\
(The model does not take into account the images)

•   How do i chose which machine to buy from X and Y.
(Missing context, too general)

### Will be able to answer

•	Tell me about the motor of machine X.

•	Tell me which button do i have to press for the Machine X to do the task Y.

•	What is the screw located in location X in the machine Y designed for.

•	How do i dissemble machine X.

•   What are the main differences between machine X and Y.


