# 📖Bestseller Prediction 
**Scenario**\
To become a bestseller, a book must record a high volume of weekly sales across diverse retailers during a single week, backed by a confidential, weighted-average formula managed by the publication. 
The book press aims to use machine learning to help them predict which books will become best sellers. 
These predictions would allow them to decide whether or not to invest in a book, or how to proceed in marketing it.

## Approach
- **Base Dataset:** Goodreads books (4400 recrods), chosen because it has the richest feature set.
- **Target Variable:** A book is labelled a bestseller if it appears in the NYT or Kindle bestseller datasets. Books were matched on author and title (as well as ISBN where applicable)
- **Features:**
  - Page count and price
  - Format, consolidated from 16 values to 3, then one-hot encoded
  - Genres, grouped into 14 broader genres and multi-label encoded
  - Fiction/non-fiction flag
  - Author popularity, 1-10 score built from average number of ratings across an author's *other* books (as a proxy), plus a single_book_author flag
- **Leakage prevention:** post-release fields were dropped as features

## Tech Stack
- Python
- Jupyter Notebook
- pandas, Numpy (data cleaning and modelling)
- scikit-learn (modelling, preprocessing, evalutation)
- matplotlib, seaborn (visualisation)

## Model
Logistic regression implemented from scratch with gradient descent. It gives each book a probability of becoming a bestseller, so the press can rakn titles and choose its own investment threshold.\
A decision tree and support vector machine were also tested for comparison

**Evaluation:** After labelling, bestsellers make up 53% of the dataset, so the classes are roughly balanced. Models were evaluated on a test set of 718 books, using confusion matrices, accuracy, precision, recall and f1 score

## Results
(metrics for bestseller class)
| Model| Accuracy | Precision | Recall | F1-Score | 
|------|----------|-----------|--------|----------|
| LR   | 0.77     | 0.82      | 0.72   | 0.77     |
| DT   | 0.74     | 0.76      | 0.75   | 0.75     |
| SVM  | 0.76     | 0.80      | 0.74   | 0.77     |

Logistic regression performed well overall, with the highest precision on the bestseller class.\
This suits the overall business goal because a false positive could mean money spent on book that doesn't deliver.
The SVM was close behind and the tree was slightly weaker. Still the gaps are small (with only 718 books) and the models perform similarly

## Model Interpretation
Both logistic regression and the decision tree identified author popularity as the dominant predictor, with genre (romance, graphic novels) and format (hardcover) playing smaller parts.\
This is consistent with my view that bestseller status is driven largely by an author's existing audience rather than by book attributes.

There are some limitations with this however; it uses the average rating of an author's other books only, so a books own rating never contributes to the score. The score is not strictly pre-release, since dates were not considered. 'single_book_author' reflects the dataset's coverage and not true debut status
