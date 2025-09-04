### **Phase 0: Foundation & Setup (Pre-MVP)**

**Objective:** Get Django installed and create the basic project structure. Understand Django's core components: Project vs. App, Models, Views, Templates (the MVT pattern).

1.  **Environment Setup:**
    * [x] Create a new directory for your project and navigate into it in your terminal.
    * [x] Create a Python virtual environment (`python -m venv crono-linguist`) and activate it.
    * [x] Install Django (`pip install django`).

2.  **Create Project and App:**
    * [x] Start a new Django project: `django-admin startproject chronolinguist`.
    * [x] Navigate into your new project directory.
    * [x] Create a new app within the project. This app will hold the core functionality: `python manage.py startapp corpus`.
    * [x] Register your new `corpus` app in the `INSTALLED_APPS` list in `chronolinguist/settings.py`.

3.  **Version Control:**
    * [x] Initialize a git repository in your project root. 
    * [ ] Create a `.gitignore` file for Python/Django. 
    * [ ] Commit this initial setup. `git pull`

---

### **Phase 1: MVP (Minimum Viable Product)**

**Objective:** A simple web app where a user can upload a single text file, and the app will display a basic analysis (e.g., Word Frequency without stop words).

**Focus:** Learning Models, Basic Views, Templates, and File Handling.

1.  **Define the Core Model:**
    *   In `corpus/models.py`, create a model called `Text` or `Document`.
    *   Key fields: `title` (CharField), `author` (CharField), `text_file` (FileField, to upload a .txt file), `uploaded_at` (DateTimeField, auto_now_add=True).
    *   Create and run migrations (`python manage.py makemigrations`, `python manage.py migrate`).

2.  **Create a Form:**
    *   In `corpus/forms.py`, create a `ModelForm` for your `Text` model. This form will be the upload interface.

3.  **Build the Views (Controller Logic):**
    *   **Upload View (`upload_text`):**
        *   In `corpus/views.py`, create a function-based view.
        *   It should handle both GET (display the empty form) and POST (process the uploaded file, save it to the model, then redirect to the analysis page).
    *   **Analysis View (`text_analysis`):**
        *   This view will take the `id` of a specific `Text` object as a parameter.
        *   It will read the content of the uploaded file.
        *   It will perform your **basic word frequency analysis** (read the text, lower case, split, remove stop words using a hard-coded list, count, sort).
        *   It will pass the top 10-20 words and their counts to the template.

4.  **Design Basic Templates:**
    *   Create a `templates` directory inside your `corpus` app.
    *   **Base template (`base.html`):** Contains the HTML boilerplate, CSS includes, a navigation bar.
    *   **Upload template (`upload.html`):** Extends the base template. Renders the upload form.
    *   **Analysis template (`analysis.html`):** Extends the base template. Displays the title of the text and a simple table or list of the most frequent words.

5.  **Configure URLs:**
    *   In `corpus/urls.py`, define URL patterns that map to your views (e.g., `path('upload/', views.upload_text, name='upload')`, `path('analysis/<int:text_id>/', views.text_analysis, name='analysis')`).
    *   Include these `corpus.urls` in the main project's `urls.py`.

**MVP Goal Achieved:** You can upload a file and see a basic word frequency list. You are now using Django's core components.

---

### **Phase 2: Beta Version**

**Objective:** Expand analysis features and improve the user interface. Introduce the "Dossier" concept and the stop words toggle.

**Focus:** More complex views, form enhancements, interactive elements, and database relationships.

1.  **Enhance the Model:**
    *   Create a `Dossier` model. A `Dossier` has a `name` and a `description`.
    *   Add a `ForeignKey` field from the `Text` model to the `Dossier` model. This creates a many-to-one relationship (Many Texts can be in One Dossier).
    *   Create and run migrations.

2.  **Upgrade the Upload Form:**
    *   Modify the form to include a dropdown to select an existing `Dossier` or an option to create a new one.

3.  **Implement the Stop Words Toggle:**
    *   Add a checkbox to your upload form: `Remove Stop Words?` [ ]
    *   In your analysis view, check if this box was ticked. If it was, use your stop words removal logic. If not, show the frequency list with all words.
    *   **Challenge:** Modify the analysis view to handle both cases from the same form submission.

4.  **Add New Analyses:**
    *   **Sentence Length:** In your analysis view, add logic to split the text by sentences and calculate the average sentence length. Display this statistic.
    *   **Sentiment Analysis:** Use TextBlob (`from textblob import TextBlob`) to get a polarity score for the *entire text*. Display it as "Overall Sentiment: Positive/Negative/Neutral".

5.  **Improve the UI:**
    *   Use a lightweight CSS framework like **Bulma** or **Picnic** to make the interface look decent without much effort.
    *   Display the analysis results in a more structured way (e.g., using cards or panels).

**Beta Goal Achieved:** Users can organize texts into Dossiers and perform multiple types of analysis with configurable options (stop words).

---

### **Phase 3: V1.0 (First Full Version)**

**Objective:** Introduce data visualizations, the built-in library, and more sophisticated analysis.

**Focus:** Working with external libraries (Chart.js), data management (fixtures), and more complex data processing.

1.  **Data Visualization:**
    *   Integrate **Chart.js** by including its CDN in your base template.
    *   Modify your `text_analysis` view to pass the word frequency data as JSON to the template.
    *   Write JavaScript in your `analysis.html` template to render a bar chart of the top words.

2.  **Create the Built-in Library:**
    *   Create a new Django management command (e.g., `python manage.py load_library`).
    *   Write the command to read text files from a local directory (e.g., `library/`) and create `Text` objects for them. Populate the `title` and `author` fields. You can assign them to a "Public Library" dossier.
    *   This teaches you about Django's powerful custom command system.

3.  **Advanced Analysis Views:**
    *   **Sentiment Over Time:** Create a new view `sentiment_analysis` that splits a text into chunks, calculates sentiment for each, and passes the data to Chart.js to render a line chart.
    *   **N-grams:** Create a view that calculates and displays the most common bigrams.

4.  **Stylometric Comparison (Basic):**
    *   Create a new view that takes two `Text` IDs.
    *   The view calculates a simple metric (like average sentence length or TTR) for both and displays them side-by-side for comparison.

**V1.0 Goal Achieved:** A fully functional web app with visualizations, a library of texts, and multiple analytical tools for single texts.

---

### **Phase 4: V2.0+ (Advanced Features)**

**Objective:** Implement the most complex features: cross-text analysis, topic modeling, and improved stylometrics.

**Focus:** Performance, more complex algorithms, and advanced Django features (maybe REST APIs, background tasks).

1.  **Cross-Text Analysis Dashboard:**
    *   Create a view for a `Dossier` that displays comparative metrics for all texts within it (e.g., a table of average sentence length, lexical diversity, sentiment).

2.  **Topic Modeling (LDA):**
    *   This is computationally heavy. Research `gensim` library.
    *   **Warning:** This might be too slow to run in a web request. This is where you'd learn about **asynchronous tasks** with **Celery**.

3.  **Advanced Stylometry:**
    *   Move beyond simple metrics. Train a model on function word frequency to perform author attribution, as you described.

4.  **User Authentication:**
    *   Implement Django's built-in authentication system. Users should only see their own Dossiers and uploaded texts.

This phased approach will give you a solid, working application at every step and systematically introduce you to Django's features. Good luck, and enjoy the process of building