# shopping-agent
# 🛒 AI Shopping Assistant

An **Agentic AI-powered shopping assistant** that helps users discover products, compare them using customer ratings, search for similar products from images, and place orders through a conversational interface.

The application combines **LLMs, tool calling, computer vision, LangChain, Streamlit, and SQLite** to create an end-to-end AI shopping workflow.

---

## ✨ Features

### 💬 Natural Language Product Search

Users can describe what they want in natural language, for example:

> "I want organic honey under $20 with a 4.5+ rating."

The AI agent understands the requirements and searches the product database accordingly.

### 🔍 Intelligent Product Filtering

Products can be filtered based on:

* Product name
* Category
* Description
* Maximum price
* Organic/non-organic status
* Minimum customer rating

The product search tool queries the SQLite database and returns matching products.

### ⭐ Customer Rating Analysis

The assistant retrieves:

* Average product rating
* Number of reviews

Ratings are calculated dynamically from the `reviews` table in the database.

### 🖼️ Shop by Image

Users can upload a product image through the Streamlit interface.

The vision model analyzes the image and extracts:

* Product type
* Search keywords
* Organic status
* Product description

These attributes are then used to search for similar products in the store.

### 🛍️ Conversational Ordering

After viewing the available products, users can explicitly confirm their choice.

The agent then places the order and stores it in the database.

**The system does not place an order until the user explicitly confirms it.**

### 🗄️ SQLite Database

The project uses SQLite to store:

* Products
* Reviews
* Orders

The database schema is initialized through `setup_db.py`.

---

## 🧠 Agentic AI Workflow

The project follows a tool-based agentic workflow:

```text
                    ┌──────────────────┐
                    │      User        │
                    └────────┬─────────┘
                             │
                 Natural Language / Image
                             │
                             ▼
                 ┌─────────────────────┐
                 │   AI Shopping Agent │
                 │   LangChain + LLM   │
                 └──────────┬──────────┘
                            │
             ┌──────────────┼──────────────┐
             │              │              │
             ▼              ▼              ▼
      Search Products   Get Ratings   Analyze Image
             │              │              │
             └──────────────┼──────────────┘
                            │
                            ▼
                    Product Results
                            │
                            ▼
                  User Confirmation
                            │
                         "Yes"
                            │
                            ▼
                       Checkout
                            │
                            ▼
                    Order in SQLite
```

The agent uses four main tools:

1. `search_products`
2. `get_rating`
3. `checkout`
4. `describe_product_image`

---

## 🏗️ Project Architecture

```text
User
 │
 ▼
Streamlit UI
 │
 ▼
LangChain Agent
 │
 ├── Product Search ───────► SQLite
 │
 ├── Rating Tool ──────────► Reviews Table
 │
 ├── Vision Tool ──────────► Groq Vision LLM
 │
 └── Checkout Tool ────────► Orders Table
```

---

## 📁 Project Structure

```text
AI-Shopping-Assistant/
│
├── app.py
├── shopping_agent.py
├── reviews_api.py
├── setup_db.py
├── store.db
├── requirements.txt
├── .env
└── README.md
```

### File Description

| File                | Purpose                                                  |
| ------------------- | -------------------------------------------------------- |
| `app.py`            | Streamlit frontend and chat interface                    |
| `shopping_agent.py` | AI agent, LLMs, tools, image analysis and checkout logic |
| `reviews_api.py`    | Retrieves product ratings and review counts              |
| `setup_db.py`       | Creates and populates the SQLite database                |
| `store.db`          | Stores products, reviews and orders                      |

The Streamlit application provides both conversational shopping and image-based product search.

---

## 🛠️ Tech Stack

### AI / Agentic AI

* **LangChain**
* **LangChain Agents**
* **Groq**
* **Qwen/Qwen3-32B**
* **Llama 4 Scout 17B**
* Tool Calling
* Vision-Language Model

### Backend

* Python
* SQLite
* SQL

### Frontend

* Streamlit

### Libraries

* `langchain`
* `langchain-core`
* `langchain-groq`
* `python-dotenv`

---

## 🤖 Models Used

### Text Model

```text
qwen/qwen3-32b
```

Used as the primary reasoning model for the shopping agent.

### Vision Model

```text
meta-llama/llama-4-scout-17b-16e-instruct
```

Used for analyzing uploaded product images.

---

## 🔄 How the Agent Works

### 1. User Request

The user provides a shopping requirement.

Example:

```text
Find organic honey under $20 with a rating above 4.5.
```

### 2. Product Search

The agent calls `search_products()` with the appropriate filters.

### 3. Rating Retrieval

For each candidate product, the agent calls `get_rating()` to retrieve its average rating and review count.

### 4. Filtering

Products that don't satisfy the user's minimum rating or other requirements are removed.

### 5. Recommendation

The qualifying products are presented to the user with:

* Product name
* Product ID
* Price
* Rating
* Organic status

### 6. User Confirmation

The agent waits for an explicit confirmation.

### 7. Checkout

Once confirmed, the `checkout()` tool creates an order in the database and returns an order confirmation.

---

## 🖼️ Image Search Workflow

```text
Upload Product Image
        │
        ▼
Vision Model
        │
        ▼
Extract Product Attributes
        │
        ├── Product Type
        ├── Search Query
        ├── Organic Status
        └── Description
        │
        ▼
Search Product Database
        │
        ▼
Find Similar Products
```

The Streamlit interface supports JPG, JPEG, PNG and WEBP product images.

---

## 🗃️ Database Design

The SQLite database contains three primary tables:

### Products

Stores:

* Product ID
* Name
* Category
* Price
* Description
* Organic status

### Reviews

Stores:

* Review ID
* Product ID
* Rating
* Reviewer name
* Review text

### Orders

Stores:

* Order ID
* Product ID
* Product name
* Price
* Order timestamp

These tables and their relationships are initialized in `setup_db.py`.

---

## 🚀 Installation & Setup

### 1. Clone the Repository

```bash
git clone <your-repository-url>
cd AI-Shopping-Assistant
```

### 2. Create a Virtual Environment

```bash
python -m venv venv
```

### 3. Activate the Environment

**Windows:**

```bash
venv\Scripts\activate
```

**macOS/Linux:**

```bash
source venv/bin/activate
```

### 4. Install Dependencies

```bash
pip install -r requirements.txt
```

### 5. Configure Environment Variables

Create a `.env` file:

```env
GROQ_API_KEY=your_groq_api_key
```

### 6. Initialize the Database

```bash
python setup_db.py
```

This creates the SQLite database and populates the products and reviews.

### 7. Run the Application

```bash
streamlit run app.py
```

The application will open in your browser.

---

## 💡 Example Queries

Try asking:

```text
Find organic honey under $20.
```

```text
Show me products with a rating above 4.5.
```

```text
I want organic products under $15.
```

```text
Find me a healthy snack under $10.
```

You can also upload a product image and ask the assistant to find similar products.

---

## 🔐 Safe Ordering Flow

The agent is designed to separate **product discovery** from **purchase execution**.

It first recommends products and waits for the user to explicitly confirm the purchase.

```text
Search → Recommend → User Confirms → Checkout
```

This prevents accidental orders and ensures that checkout is only triggered after explicit confirmation.

---

## 🎯 Key Agentic AI Concepts Demonstrated

This project demonstrates practical implementation of:

* **AI Agents**
* **LLM reasoning**
* **Tool calling**
* **Function execution**
* **Multi-step workflows**
* **Vision-language models**
* **Database interaction**
* **Conversational AI**
* **Stateful chat**
* **Human-in-the-loop confirmation**
* **Automated decision workflows**

---

## 🔮 Future Enhancements

Potential improvements include:

* 🛒 Shopping cart functionality
* 👤 User accounts and authentication
* 💳 Payment gateway integration
* 📦 Real-time order tracking
* 🔎 Semantic/vector product search
* 🧠 Personalized recommendations
* 💬 Product review summarization
* 🌐 Integration with real e-commerce APIs
* 📊 Purchase history and analytics
* 🎙️ Voice-based shopping
* 🌍 Multilingual shopping assistant

---

## 👩‍💻 Author

**Ridhima Sharma**

B.Tech Computer Science & Engineering — AI/ML

Interested in **Artificial Intelligence, Machine Learning, Agentic AI and Generative AI**.

---

## ⭐ If you found this project interesting

Give the repository a ⭐ and feel free to explore, fork, and build upon it!
