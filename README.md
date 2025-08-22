# recsys


Recommender system

- **Github repository**: <https://github.com/praveenhm/recsys/>
- **Documentation** <https://praveenhm.github.io/recsys/>

## Getting started with your project


### 2. Set Up Your Development Environment

Then, install the environment and the pre-commit hooks with

```bash
make install
```

This will also generate your `uv.lock` file

### 3. Run the pre-commit hooks

Initially, the CI/CD pipeline might be failing due to formatting issues. To resolve those run:

```bash
uv run pre-commit run -a
```

### 4. Commit the changes

Lastly, commit the changes made by the two steps above to your repository.

```bash
git add .
git commit -m 'Fix formatting issues'
git push origin main
```

You are now ready to start development on your project!
The CI/CD pipeline will be triggered when you open a pull request, merge to main, or when you create a new release.

To finalize the set-up for publishing to PyPI, see [here](https://fpgmaas.github.io/cookiecutter-uv/features/publishing/#set-up-for-pypi).
For activating the automatic documentation with MkDocs, see [here](https://fpgmaas.github.io/cookiecutter-uv/features/mkdocs/#enabling-the-documentation-on-github).
To enable the code coverage reports, see [here](https://fpgmaas.github.io/cookiecutter-uv/features/codecov/).

## Releasing a new version

Repository initiated with [fpgmaas/cookiecutter-uv](https://github.com/fpgmaas/cookiecutter-uv).


# BigQuery Item-to-Item Recommender for E-Commerce

This project contains a complete, scalable, and efficient implementation of an item-to-item collaborative filtering model ("Customers who bought this also bought...") built entirely on Google BigQuery SQL. It is designed to provide product recommendations for an e-commerce marketplace without requiring external processing or machine learning platforms.

## 1. The Problem: Predicting the Next Purchase

For any e-commerce platform, a key driver of engagement and sales is the ability to show users relevant products. A powerful way to do this is by displaying "also bought" recommendations on product pages.

The primary goal of this project was to build this recommendation logic by analyzing the collective purchase history of all users. The core challenge was to do this efficiently at scale, using only BigQuery SQL on a dataset with millions of daily transactions.

## 2. The Solution: A Purely SQL-Based Model

The model is built using a statistical method known as **Association Rule Mining**. We calculate a "confidence score" for every pair of items, which answers the question:

> What is the probability that a customer will buy **Item B**, given that they have already purchased **Item A**?

This is expressed as `p(B|A) = (Users who bought both A & B) / (Total users who bought A)`.

The entire process is broken down into a multi-step SQL pipeline that refines the data and builds the final recommendation model.

### The SQL Pipeline

The logic is executed in a series of sequential BigQuery queries that create permanent tables, making the process auditable and modular.

#### **Step 1: Create a Clean Foundational Dataset (`buyer_item`)**
- **Purpose:** To create a clean, reliable dataset of all valid user-item purchases from the last 30 days.
- **Source:** Raw, date-sharded transaction tables (`imported_view_items_updated_*`).

#### **Step 2: Calculate Item Popularity (`item_total_purchases`)**
- **Purpose:** To calculate the total number of unique buyers for every single item. This serves as the denominator for our confidence score calculation.

#### **Step 3: Build the Core Co-Purchase Logic (`item_co_purchase_counts`)**
- **Purpose:** To find every pair of items that were purchased by the same user and to count how many users bought both. This is the most computationally intensive step in the pipeline.

#### **Step 4: Generate the Final Recommendation Model (`item_recommendations`)**
- **Purpose:** To create the final, reusable lookup table. It joins the results from Step 2 and Step 3 to calculate the final `confidence_score` for every item pair, effectively completing the model.

## 3. Key Challenges & Solutions

Building this model at scale within the constraints of SQL presented several significant technical hurdles.

| Challenge | Solution |
| :--- | :--- |
| **Querying Massive, Date-Sharded Tables** | The initial queries were slow and expensive. This was solved by using BigQuery's **wildcard tables** (`table_*`) and filtering on the `_TABLE_SUFFIX` pseudo-column to scan only the required date range (e.g., last 30 days). |
| **Combinatorial Explosion on Join** | A standard `SELF JOIN` to create item pairs failed to complete, as it exhausted system resources. The solution was to switch to an array-based approach, aggregating all items per user into an array first. |
| **UDF Out of Memory Error** | Even the array approach failed due to a single **"super user"** (reseller) with over 67,000 purchases. This was definitively solved by implementing a **JavaScript UDF** to generate pairs efficiently and adding a `HAVING` clause to exclude these outliers from the model, which also improved recommendation quality. |
| **Null Results from Data Sparsity** | The final model initially produced no results. Diagnostics revealed that the `WHERE total_buyers > 5` filter was too strict for the sparse 30-day dataset. **Adjusting the filter** to `> 1` solved the issue and produced a high-quality final result. |

## 4. How to Use the Recommender

With the `item_recommendations` table built, getting the top 10 recommendations for any item is fast and simple.

```sql
-- Get top 10 'also bought' items for a specific product
SELECT
  recommended_item,
  confidence_score
FROM
  `auxia-reporting.temp_dataset_rec.item_recommendations`
WHERE
  source_item = 'm38585425106' -- <-- Just change this item_id
ORDER BY
  confidence_score DESC
LIMIT 10;
```

## 5. Next Steps

- **Scale to a 6-Month Window:** Re-run the optimized pipeline on a larger dataset to capture more purchase patterns and improve recommendation quality.
- **Automation:** Schedule the entire SQL pipeline to run on a weekly or monthly basis to keep the recommendations fresh.
- **A/B Testing:** Plan and implement a live A/B test to measure the business impact (CTR, conversion rate) of showing these recommendations to users.