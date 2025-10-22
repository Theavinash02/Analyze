# Sales Data Analysis & Reporting

This repository contains a Python application for processing sales data, performing key analytical calculations, and publishing the results. The project demonstrates a robust CI/CD pipeline using GitHub Actions to automate data processing and report generation, making the latest insights available via GitHub Pages.

## Project Summary

The core of this project is `execute.py`, a Python script designed to read sales transaction data, calculate various metrics, and output them in a structured JSON format. Key functionalities include:

*   Calculating total row count.
*   Determining the count of distinct sales regions.
*   Identifying the top N (configurable, default N=3) products by total revenue.
*   Computing the last known 7-day rolling average of daily revenue for each region.

The data source, originally `data.xlsx`, is converted to `data.csv` for efficiency and compatibility. A GitHub Actions workflow automates the execution of `execute.py`, ensuring code quality with `ruff`, and publishing the generated `result.json` to GitHub Pages upon every push to the repository.

## Setup Instructions

This project is primarily designed for automated execution and reporting.

### Viewing the Latest Report

The `result.json` file, containing the latest analysis, is automatically generated and published via GitHub Pages. You can view the live report by visiting:

`https://[YOUR_USERNAME].github.io/[YOUR_REPOSITORY]/result.json`

*(Replace `[YOUR_USERNAME]` and `[YOUR_REPOSITORY]` with your actual GitHub username and repository name.)*

### Local Development and Execution

If you wish to run the script locally or contribute to the project:

1.  **Clone the Repository:**
    ```bash
    git clone https://github.com/[YOUR_USERNAME]/[YOUR_REPOSITORY].git
    cd [YOUR_REPOSITORY]
    ```

2.  **Create a Virtual Environment (Recommended):**
    ```bash
    python -m venv .venv
    source .venv/bin/activate # On Windows use `.\.venv\Scripts\activate`
    ```

3.  **Install Dependencies:**
    ```bash
    pip install pandas ruff
    ```
    *Ensure you are using Python 3.11+ and Pandas 2.3+ for full compatibility.*

4.  **Run the Analysis Script:**
    ```bash
    python execute.py > result.json
    ```
    This will generate `result.json` in your local directory with the computed metrics.

## Code Explanation

### `execute.py`

This Python script orchestrates the data analysis.

*   **Data Ingestion**: It reads sales data from `data.csv`. *(Initially, the project was provided with `data.xlsx`, but it has been converted to `data.csv` and the script updated accordingly).*
*   **Revenue Calculation**: Computes `revenue` for each transaction (`units` * `price`).
*   **Key Metrics**:
    *   `row_count`: Total number of records in the dataset.
    *   `regions`: Count of unique sales regions.
    *   `top_n_products_by_revenue`: Identifies the top 3 products based on their aggregated revenue.
    *   `rolling_7d_revenue_by_region`: Calculates the 7-day rolling average of daily revenue for each region, returning the last available value.
*   **Bug Fixes**: A critical error in the original script where `revenew` was mistakenly used instead of `revenue` during the daily revenue aggregation has been corrected to ensure accurate rolling average calculations.
*   **Output**: The script prints the calculated metrics to standard output in JSON format, which is then redirected to `result.json` by the CI workflow.

### `data.csv`

This file serves as the primary data source for `execute.py`. It is a comma-separated values (CSV) file, converted from the original `data.xlsx` for streamlined processing.

### `.github/workflows/ci.yml`

This GitHub Actions workflow automates the entire process:

*   **Trigger**: Runs automatically on every `push` to the repository.
*   **Environment**: Sets up a Python 3.11+ environment with Pandas 2.3+ installed.
*   **Linting**: Executes `ruff` to ensure code quality and adherence to style standards. The results are visible in the CI log.
*   **Execution**: Runs `python execute.py` and redirects its output to `result.json`.
*   **Publication**: Utilizes `actions/upload-pages-artifact` and `actions/deploy-pages` to publish the generated `result.json` file to GitHub Pages, making it publicly accessible.

## License

This project is licensed under the MIT License. See the `LICENSE` file for details.