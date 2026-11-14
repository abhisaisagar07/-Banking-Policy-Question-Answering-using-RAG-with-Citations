# -Banking-Policy-Question-Answering-using-RAG-with-Citations
Banking Policy Question Answering using RAG with Citations
Domain: Finance

1. Simple meaning
A banking chatbot that answers questions only from real banking policy documents — and with every answer, it shows the proof: which document, which page, which line it took the answer from. That proof is called a citation.

Example:

User: What is the minimum balance for a savings account?
Bot: The minimum balance is ₹1,000 for metro branches.
Source: Savings Account Policy.pdf, Page 3, Section 2.1

2. Problem it solves
A normal chatbot (like plain ChatGPT/Gemini) can make up wrong banking rules — this is called hallucination. In banking, a wrong answer about fees, loans or KYC is dangerous. Nobody can trust it.

Banking Policy Question Answering using RAG with Citations solves this: the bot is not allowed to answer from its own memory. It must first search the policy documents, answer only from what it finds, and show the source. If the answer is not in the documents, it must say "I don't have this information in the policy documents."

3. What is RAG? (the core concept)
RAG = Retrieval-Augmented Generation.

3 simple steps:
Retrieve — search the documents for the parts related to the question
Augment — give those parts to the LLM along with the question
Generate — the LLM writes the answer using only those parts
Flow:



Policy PDFs → cut into small pieces (chunks) → convert to numbers (embeddings) → store in Vector DB
                                                                                      ↓
User question → search Vector DB → top 3-4 matching chunks → give to Gemini → answer + citation

4. How you will build it — step by step
Collect documents — 4–5 sample banking policy PDFs: Savings Account, KYC Rules, Personal Loan, Credit Card Charges, Fixed Deposit. You can take public policy pages from bank/RBI websites, or we create clean sample PDFs. No real customer data — only policy rules.
Chunking — split each PDF into small pieces (about 500 words each, with a little overlap), keeping the page number with every piece. This page number is what becomes the citation later.
Embeddings + Vector DB — convert each piece into a vector (list of numbers that captures meaning) and store in ChromaDB (free, easy on Windows).
Retrieval — when a question comes, convert it to a vector too, and find the 3–4 closest pieces.
Answer + Citation — send question + those pieces to Gemini with a strict instruction: "Answer only from this context. Add the document name and page. If not found, say you don't know."
Simple UI — a Streamlit chat page: question box on top, answer in the middle, sources shown below it.
Testing — prepare 20 test questions. Check 3 things: Is the answer correct? Is the citation correct? For a question not in the documents, does it refuse instead of guessing?


5. Tools 
Python — main language
LangChain — connects all the RAG steps
ChromaDB — vector database
Gemini — the LLM that writes the final answer
PyPDF — reads the PDF files
Streamlit — the chat interface
