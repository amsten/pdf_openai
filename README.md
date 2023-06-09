# Playing with OpenAI
This Python script is intended to extract text from a PDF file named "bitcoin.pdf" located in the "data" directory. The script leverages the `PdfReader` class from a library, most likely `pdfplumber` or `pdfreader`.

Here's the line-by-line breakdown:

1. `reader = PdfReader("data/bitcoin.pdf")`: This line creates an instance of `PdfReader`, which opens and reads the contents of the PDF file specified in the parentheses.

2. `number_of_pages = len(reader.pages)`: This line gets the total number of pages in the PDF file by using the `len()` function on `reader.pages`.

3. `raw_text = ""`: This line initializes an empty string variable named `raw_text`. This will be used to store all the text extracted from the PDF file.

4. `for idx, page in enumerate(reader.pages):`: This line initiates a `for` loop. The `enumerate()` function is used on `reader.pages` to allow the loop to keep track of the current page number (`idx`) along with the page contents (`page`).

5. `raw_text += page.extract_text()`: Inside the loop, this line extracts the text from the current page using the `extract_text()` method and appends it to the `raw_text` string.

6. `if idx % 10 == 0:`: This line is the start of an `if` statement which checks if the current page number (`idx`) is a multiple of 10. The `%` operator is the modulus operator, which returns the remainder when `idx` is divided by 10. If the remainder is 0, that means `idx` is a multiple of 10.

7. `print(f"Processed {idx} pages of {number_of_pages}")`: If the condition in the `if` statement is true, then this line of code is executed. It prints a progress message to the console indicating how many pages (multiples of ten) have been processed so far, out of the total number of pages.

This code provides a way to monitor the progress of the text extraction process, particularly useful when dealing with large PDF files as it provides updates every 10 pages.


## Sources: 
https://python.langchain.com/en/latest/modules/indexes/vectorstores/examples/faiss.html
https://platform.openai.com/docs/api-reference/models


## Setup
Clone the project.
Run the commands: 
```console
python -m venv env
source env/bin/activate
pip install -r requirements.txt
echo 'OPENAI_API_KEY=' >.env
```

Add your own API key to the .env file.

# Increasing efficiency and keeping costs down

1. **Limit the pages processed**: Depending on the nature of the documents you are processing, you may not need to process all the pages. Some documents, for example, might have useful information concentrated in certain sections, while other parts like legal disclaimers or appendixes are less relevant. You could modify the script to focus on certain sections or pages of the document, thus reducing the computational resources required.

2. **Incremental processing and storing results**: Depending on the frequency of updates to your documents and the questions asked, it might be more efficient to store the results of the OpenAI processing and similarity search. That way, if the same question is asked again or the same document is processed, you can save both time and cost of reprocessing. 

3. **Use bulk API calls**: When calling APIs, sending requests in bulk (batching) is usually more efficient than sending individual requests. This is because the overhead of setting up the connection and transferring the data is amortized over many items. Check if the APIs used (like OpenAI API) support batch requests, and if so, revise the code to leverage this.

4. **Optimize text splitting**: The choice of chunk size when splitting the document can significantly impact both the quality of the answers and the cost. Smaller chunks might lead to missing context necessary for answering some questions, while larger chunks will cost more to process. You may need to experiment with different chunk sizes to find the optimal balance for your specific use case.

5. **Prune unnecessary questions**: If the code is being used in an interactive setting where users can ask any question, consider adding a preliminary step to weed out irrelevant or unanswerable questions. This might involve using a simpler (and cheaper) natural language processing tool to first analyze the question, or even a basic keyword check.

6. **Asynchronous processing**: If processing time is a bottleneck and you are processing multiple documents or questions, you could consider using asynchronous processing or even distributing the workload across multiple machines. 

Remember that any optimization should be driven by the specific requirements and constraints of your use case. Also, always monitor the costs and performance to ensure that the optimizations are having the desired effect.

