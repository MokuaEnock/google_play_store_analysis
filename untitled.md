# What you submit

You upload **one file** to the assignment: your completed Jupyter Notebook (`.ipynb`). But the project produces **three things**, and two of them live inside or alongside that notebook.

## 1. The Jupyter Notebook (the only file you upload)

Start from your `play_store_analysis.ipynb` template and complete every section:

| Section | What to complete |
|---|---|
| **Banner, title, overview** | Header image, a project title, and a short BLUF paragraph summarizing your findings |
| **Business Understanding** | Why this matters, stakeholders (developers, product managers), and the questions you'll answer |
| **Data Understanding** | Data source, column descriptions, what you'll keep or drop, expected cleaning issues |
| **Imports cell** | PySpark, Pandas, matplotlib/seaborn, etc. (currently says `None`) |
| **EDA code** | Schema, counts, `describe()`, nulls, duplicates, distributions, outliers, plus a written list of data issues |
| **Data Preparation** | Cleaning in PySpark first, then Pandas, plus validation checks |
| **Save cleaned data cell** | Export the cleaned CSV |
| **Data Analysis** | Notebook charts and the insights from them |
| **Link to Published Dashboard** | Paste your Tableau Public URL |
| **Conclusion** | Findings, limitations, and recommendations |
| **AI documentation** | Prompts you used, AI responses, and any changes you made (add this section yourself) |

Also make sure the notebook has **outputs showing** (run all cells before saving), and replace every `None` placeholder.

## 2. The cleaned CSV (a working file, probably not uploaded)

- Created by your notebook's "Save cleaned data" cell.
- You load it into Tableau as the data source.
- Verify it: reload it and check row count and no unexpected nulls.
- The assignment says to submit the notebook only, but check whether your instructor also wants the CSV. If unsure, ask or include it only if allowed.

## 3. The Tableau Public dashboard (submitted as a link inside the notebook)

- Minimum **6 core visualizations**, each tied to a business question.
- **Multi-page** dashboard with navigation between pages.
- Consistent theme, **filters and parameters** for interactivity.
- **Publish to Tableau Public** and paste the link under "Link to Published Dashboard".
- Test the link in a private/incognito window to confirm it's public.

## Suggested order of work

1. Get the dataset, set up Spark, load the data.
2. Write the business context.
3. Run EDA and list the data issues.
4. Clean, validate, and export the CSV.
5. Prototype charts in the notebook.
6. Build and publish the Tableau dashboard.
7. Add the link, write the conclusion and AI documentation.
8. Run all cells and submit the `.ipynb`.

**Quick check:** the assignment text says to submit "your Jupyter Notebook project file," so the notebook plus the embedded Tableau link is the full submission.

Want me to start on the PySpark setup and loading code, or the cleaning functions?