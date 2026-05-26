# Book Recommender System

A two-in-one book recommendation system — a **popularity-based engine** for trending books and a **collaborative filtering engine** for personalized recommendations — built on 1.1M+ real user ratings and deployed with a Flask web interface.

---

## 📌 What This Project Does

Takes a book title as input and returns the **top 5 most similar books** based on how real users have rated them — not just genre tags or author matching. Also surfaces a curated list of the **top 50 most popular books** filtered by both rating count and average score.

For example: input `1984` → returns `Animal Farm`, `The Handmaid's Tale`, `Brave New World`, `The Vampire Lestat`, `The Hours`

---

## ⚙️ Tech Stack

| Category | Tools |
|---|---|
| Language | Python |
| Data Processing | Pandas, NumPy |
| Recommendation | Scikit-learn (Cosine Similarity) |
| Web App | Flask |
| Frontend | HTML, CSS |
| Serialization | pickle |

---

## 📊 Dataset

**Amazon Book-Crossing Dataset** — 3 files:

| File | Records | Details |
|---|---|---|
| Books.csv | 271,360 books | Title, author, year, publisher, cover image URL |
| Users.csv | 278,858 users | User ID, location, age |
| Ratings.csv | 1,149,780 ratings | Explicit ratings (1–10) and implicit (0) |

---

## 🧠 How It Works

The system has two separate engines:

### Engine 1 — Popularity-Based Recommender
Surfaces the most well-known and highly rated books for new users with no history.

**Pipeline:**
1. Merged ratings with book metadata on ISBN
2. Computed `num_rating` (total ratings per book) and `avg_rating` (mean score per book)
3. Filtered to books with **minimum 250 ratings** — removes obscure/low-data titles
4. Sorted by `avg_rating` descending → kept **top 50 books**
5. Merged with book metadata for title, author, and cover image

**Result:** 50 popular books with verified rating volume and quality scores

---

### Engine 2 — Collaborative Filtering Recommender
Finds books similar to a given title based on shared user rating patterns.

**Pipeline:**
1. Filtered to **811 active users** who rated 200+ books — ensures reliable rating signal
2. Filtered to **706 books** rated by at least 50 of those active users — reduces sparsity
3. Built a **pivot table** (706 books × 810 users) with ratings as values, NaN filled with 0
4. Computed **cosine similarity** across all book vectors — produces a 706×706 similarity matrix
5. `recommend(book_name)` function finds the book's index → sorts similarity scores → returns top 5

**Why cosine similarity?**
Two books are similar if the same users rated them similarly — regardless of rating scale differences between users. Cosine similarity captures directional agreement in rating patterns, not absolute values.

---

## 📁 File Structure

```
book-recommender-system/
├── app.py                        # Flask web app
├── book-recommender-system.ipynb # Full data processing + model notebook
├── popular.pkl                   # Saved popular books dataframe
├── pt.pkl                        # Saved pivot table (706 × 810)
├── books.pkl                     # Saved books metadata
├── similarity_scores             # Saved cosine similarity matrix
├── templates/                    # HTML templates
└── static/                       # CSS and frontend assets
```

---

## 🚀 How to Run Locally

```bash
# 1. Clone the repo
git clone https://github.com/Kirtisharma2000/book-recommender-system.git
cd book-recommender-system

# 2. Install dependencies
pip install flask pandas numpy scikit-learn

# 3. Run the app
python app.py
```

Then open `http://localhost:5000` in your browser.

> **Note:** The dataset CSV files (Books.csv, Users.csv, Ratings.csv) are not included due to size. Download them from the [Book-Crossing Dataset on Kaggle](https://www.kaggle.com/datasets/arashnic/book-recommendation-dataset) and place them in the root folder before running the notebook.

---

## 📊 Key Numbers

| Metric | Value |
|---|---|
| Total ratings processed | 1,149,780 |
| Total books | 271,360 |
| Total users | 278,858 |
| Active users (filter: 200+ ratings) | 811 |
| Books in collaborative model (filter: 50+ ratings) | 706 |
| Popular books shown | Top 50 (min 250 ratings) |
| Recommendations per query | Top 5 |
