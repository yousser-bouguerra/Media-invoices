Invoice digitization platform with AI processing, built as my final-year project (PFE) at MediaSoft Tunisie.

Users upload an invoice (from the web or by scanning a QR code with their phone). The platform extracts the data with OCR, runs it through an AI agent that validates, classifies and checks it for fraud, then stores it and keeps the user informed in real time. Invoices can be synchronized with Divalto ERP.

Demo





Users

4-step invoice upload stepper (file, OCR extraction, validation, confirmation).
Mobile upload through a QR code: scan on the desktop page, take the photo with the phone, the invoice appears on the desktop.
Validation pages to review and correct the extracted data.
Real-time notifications when an invoice status changes (Signal)
).
Dashboards for admin and user roles.

AI

LangGraph agent in Node.js: a 5-node pipeline (extraction, validation, classification, anti-fraud, storage).
OCR with Tesseract and ImageSharp, with a parser that handles two invoice formats (Divalto and Mediasoft).
Chatbot powered by the OpenAI API.
RAG pipeline with Ollama: embeddings of the invoice data, cosine similarity search, answers about the invoices.

Platform

JWT authentication, BCrypt password hashing, role-based access (administrator and user).
Automated synchronization flow with Divalto ERP.
Architecture
Angular front end  <--REST / SignalR-->  ASP.NET Core API  <--->  SQL Server
                                              |
                                              +--> LangGraph agent (Node.js / Express, POST /analyze-invoice)
                                              +--> Ollama (embeddings + RAG, POST /api/rag/ask)
                                              +--> OpenAI API (chatbot)
                                              +--> Divalto ERP (synchronization)
Part	Technologies
Front end	Angular, TypeScript, RxJS, standalone components
Backend API	ASP.NET Core, C#, Entity Framework Core, SQL Server, JWT, SignalR
AI service	Node.js, Express, LangGraph
OCR	Tesseract, ImageSharp
LLM	OpenAI API, Ollama (RAG)
Tools	Git, GitFlow, Scrum
The AI pipeline

The ASP.NET Core backend calls the Node.js service on /analyze-invoice. The LangGraph StateGraph runs five steps in order:

Extraction of the invoice fields from the OCR text.
Validation of the extracted data.
Classification of the invoice.
Anti-fraud checks.
Storage of the result through the main backend.
The RAG pipeline

Embeddings of the invoice data are stored in SQL Server (as JSON). For a question, the backend computes the embedding, ranks the stored data by cosine similarity (computed in C#), and sends the best matches to the model, which answers questions about the invoices.

Getting started

Adjust the folder names and commands below to match your repository.

Prerequisites: .NET SDK, Node.js, SQL Server, Ollama (for the RAG), an OpenAI API key.

bash
# 1. Backend API
cd backend
dotnet ef database update
dotnet run

# 2. AI service
cd ai-service
npm install
npm start

# 3. Front end
cd frontend
npm install
npm start      # http://localhost:4200

Configuration: set the connection string, JWT secret and OPENAI_API_KEY in your local settings or environment variables. Never commit real keys or real invoice data.

How I worked
Sole developer on the project and Scrum Master: 3 releases, 6 to 8 sprints, user stories with acceptance criteria.
Requirements analysis and UML modeling (use case and sequence diagrams) before implementation.
Git with GitFlow, technical documentation.
Limitations and roadmap
[ ] Automated tests for the OCR parser and the API
[ ] Dedicated vector database instead of embeddings stored in SQL Server (for example Qdrant)
[ ] Live demo deployment
Author

Yousser Bouguerra, Full-Stack Developer (Angular, Node.js / TypeScript, .NET). 
