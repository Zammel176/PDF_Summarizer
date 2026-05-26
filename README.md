# PDF Summarizer - RAG Application with Gemini Pro

A Retrieval-Augmented Generation (RAG) application built with Streamlit and Google's Gemini Pro that allows you to upload PDF files and ask questions about their content.

## Features

- **PDF Upload & Processing**: Upload any PDF file and extract text content
- **Smart Text Chunking**: Automatically splits documents into manageable chunks
- **Vector Embeddings**: Uses Google Generative AI embeddings for semantic search
- **Multi-language Support**: Supports English, French, and Arabic (easily extensible)
- **Retry Mechanism**: Built-in retry logic with exponential backoff for API calls
- **Chat Interface**: Interactive chat interface powered by Streamlit
- **Context-Aware Responses**: Uses retrieval-augmented generation for accurate answers

## Setup Instructions

### 1. Clone the Repository

```bash
git clone https://github.com/Zammel176/PDF_Summarizer.git
cd PDF_Summarizer
```

### 2. Create a Virtual Environment

```bash
python -m venv venv
```

**On Windows:**

```bash
venv\Scripts\activate
```

**On macOS/Linux:**

```bash
source venv/bin/activate
```

### 3. Install Dependencies

```bash
pip install -r requirements.txt
```

### 4. Set Up Environment Variables

Create a `.env` file in the project root and add your Google API key:

```
GOOGLE_API_KEY=your_google_api_key_here
```

**Get your API key:**

- Visit [Google AI Studio](https://aistudio.google.com/app/apikey)
- Create a new API key
- Copy and paste it in your `.env` file

### 5. Run the Application

```bash
streamlit run app.py
```

The app will open in your browser at `http://localhost:8501`

## Usage

1. **Upload a PDF**: Click "Upload a PDF file" and select your document
2. **Ask Questions**: Type your question in the chat input
3. **Get Answers**: The AI will search the document and provide relevant answers
4. **Chat History**: Your conversation history is maintained during the session

## Requirements

- Python 3.8+
- Streamlit
- LangChain
- Google Generative AI
- FAISS (Facebook AI Similarity Search)
- python-dotenv
- langdetect
- PyPDF
- NumPy
- Tenacity (for retry logic)

See `requirements.txt` for all dependencies and versions.

## How It Works

1. **PDF Loading**: PyPDFLoader extracts text from uploaded PDF
2. **Text Splitting**: RecursiveCharacterTextSplitter chunks the text (1000 chars per chunk)
3. **Embedding**: Text chunks are converted to vectors using Google's embedding model
4. **Vector Store**: FAISS stores embeddings for fast similarity search
5. **Retrieval**: When you ask a question, relevant chunks are retrieved
6. **Generation**: Gemini Pro generates an answer based on retrieved context
7. **Multi-language**: Questions are detected and answered in the same language

## Configuration

You can modify these settings in `app.py`:

- **Chunk Size**: Line 48 - `chunk_size=1000`
- **Batch Size**: Line 51 - `batch_size=5`
- **Embedding Model**: Line 72 - `model="models/text-embedding-004"`
- **Search Results**: Line 104 - `search_kwargs={"k": 10}` (number of chunks to retrieve)
- **Model**: Line 106 - `model="gemini-1.5-pro"`
- **Max Tokens**: Adjust in model initialization
- **Languages**: Add more language prompts in the `system_prompts` dictionary

## Supported Languages

Currently supports:

- English (en)
- French (fr)
- Arabic (ar)

To add more languages, add a new entry to the `system_prompts` dictionary in `app.py`.

## Error Handling

- **API Errors**: Automatic retry with exponential backoff (up to 5 attempts)
- **Embedding Failures**: Continues with next batch if one fails
- **Missing API Key**: Raises error on startup with clear message
- **Empty Results**: Shows warning if no documents could be embedded

## Project Structure

```
PDF_Summarizer/
├── app.py                 # Main Streamlit application
├── requirements.txt       # Python dependencies
├── README.md             # This file
├── .env                  # Environment variables (not committed)
└── .gitignore           # Git ignore rules
```

## Troubleshooting

### "GOOGLE_API_KEY not found"

- Make sure your `.env` file exists in the project root
- Verify the key name is exactly `GOOGLE_API_KEY`
- Restart Streamlit after adding the `.env` file

### Embedding errors or timeouts

- Try uploading a smaller PDF
- Reduce batch size in the code
- Check your internet connection

### Chat history not persisting

- This is expected - chat history is cleared when you refresh the page
- This is by design for security reasons

## Limitations

- PDFs are processed entirely in memory
- Large PDFs (100+ MB) may cause memory issues
- Free tier of Google API has rate limits
- Maximum context for retrieval is 10 chunks by default

## License

MIT License - feel free to use this project for personal and commercial purposes.

## Contributing

Contributions are welcome! Feel free to:

- Report bugs
- Suggest features
- Submit pull requests

## Contact

For questions or issues, please open an issue on the [GitHub repository](https://github.com/Zammel176/PDF_Summarizer).

---

**Note**: This project requires a valid Google API key for Gemini Pro. Keep your `.env` file secure and never commit it to version control.
