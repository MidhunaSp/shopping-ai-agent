# AI Shopping Assistant

A Streamlit chat app that lets you search a small store catalog (honey, oils,
nuts, grains, tea/coffee, snacks, dairy-alternatives), filter by price /
organic / rating, place an order, and even search "by image" using a vision
model.

Built with LangChain + Groq, using `openai/gpt-oss-120b` for both chat and
image understanding (it's multimodal, so one model handles both).

> Note: the original models this project shipped with — `qwen/qwen3-32b` and
> `meta-llama/llama-4-scout-17b-16e-instruct` — were deprecated by Groq on
> June 17, 2026. `shopping_agent.py` has been updated to use
> `openai/gpt-oss-120b` instead, which Groq recommends as the replacement
> for both. If Groq deprecates this model too in the future, check
> https://console.groq.com/docs/models for the current list and swap the
> model string in `shopping_agent.py`.

## 1. Set up a virtual environment (recommended)

```bash
python -m venv venv
source venv/bin/activate      # Windows: venv\Scripts\activate
```

## 2. Install dependencies

```bash
pip install -r requirements.txt
```

## 3. Add your Groq API key

Get a free key from https://console.groq.com/keys, then:

```bash
cp .env.example .env
```

Open `.env` and paste your key:

```
GROQ_API_KEY=gsk_xxxxxxxxxxxxxxxxxxxxxxxx
```

## 4. Set up the database (already included, but you can rebuild it)

`store.db` is already in this folder, pre-populated with 32 products and
reviews. If you ever want to reset it:

```bash
python setup_db.py
```

## 5. Run the app

```bash
streamlit run app.py
```

This opens the chat UI at http://localhost:8501.

- Type things like: `I want organic honey under $15 with 4+ rating`
- Or use the sidebar to upload a product photo and click
  "Find similar products"
- Confirm with "yes" / "order number 2" etc. to place an order

## Notes

- `reviews_api.py` and `setup_db.py` are helper modules, not services you
  need to run separately — `shopping_agent.py` imports `reviews_api`
  directly, and `store.db` is used by everything via plain SQLite (no server
  needed).
- Orders are saved into the `orders` table inside `store.db`.
