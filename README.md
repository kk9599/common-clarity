CommonClarity
=============

CommonClarity transforms standardized test data into a visual, interactive reporting dashboard for teachers, to help inform and target instruction.

####The Project

Data-driven instruction has grown in popularity with the rise of standardized testing. In many districts around the country, students take standardized tests nearly every other month. These tests generate a huge amount of data, which teachers are expected to utilize to inform instruction; however, they lack the time and the tools to make sense of it.

CommonClarity makes it easy for teachers to upload their data and instantly generate a dashboard of reports, showing their students' strengths and weaknesses according to Common Core State Standards. Data are grouped in useful ways to expose patterns and track progress, so that teachers can better help students improve.

####Table of Contents
- [Technologies Used](#technologies-used)
- [How It Works](#how-it-works)
- [Product Details](#product-details)
  - [The Database](#the-database)
  - [The RESTful API](#the-restful-api)
  - [AngularJS and D3](#angularjs-and-d3)
- [Product Screenshots](#product-screenshots)
  - [Student Set-up](#student-set-up)
  - [Easy Data Import](#easy-data-import)
  - [Reporting Dashboard](#reporting-dashboard)
- [Try It Yourself!] (#try-it-yourself)

####Technologies Used

CommonClarity was written using AngularJS, D3, JavaScript, Python, Flask, HTML5, CSS3/Sass, jQuery, BeautifulSoup, SQLAlchemy, and Postgresql.

[Click here for a full dashboard view.](http://i.imgur.com/r8C9qb4.jpg)
![Reporting dashboard](/static/screenshots/all_cohorts_dashboard.png)

####How It Works

Teachers can quickly set up their classes of students in the system and easily import [the CSV files of their test data](/seed_data/test_class1_interim.csv). Those files are then parsed by matching student names and Common Core State Standards to the database and entering scores for each student according to each standard.

Reports are generated by aggregating the data across all classes, by individual class, and by individual student and generating an interactive dashboard. This dashboard shows teachers the following information:
 - 1) The top Common Core State Standards students are struggling with, and which students have not met each of those standards
 - 2) The students who are struggling the most and the percentage of standards each of those students has not met
 - 3) A high-level overview of student performance on the most recent test
 - 4) A detailed view of student performance, broken out by all tests and by Common Core State Standard
 - 5) Student performance on the most recent test, compared to the rest of the school and the district overall
 - 6) A list of the standards tested on the most recent test
 - 7) An individual class's performance on the most recent test, broken out by student
 - 8) Student improvement on the most recent test from the test prior

####Product Details

#####The Database

The Postgres database uses SQLAlchemy as its ORM and contains tables for users, cohorts, student cohorts, tests, Common Core State Standards, scores, and normed scores.

To seed the Common Core State Standards table, I scraped the standards from the CCSS website using Beautiful Soup and wrote the data into a CSV file. The data were cleaned using the script [parse.py](/data_cleaning/parse.py) and seeded into the database table using the script [standards_seed.py](/standards_seed.py).

The script [scores_seed.py](/scores_seed.py) was used to seed the tests and scores tables before the test upload function was built.

#####The RESTful API

The module [api.py](/api.py) contains the RESTful API for CommonClarity. Each function is called by a Flask route in [app.py](/app.py) and, when called, queries the server, manipulates and aggregates the data, and converts the response into JSON to display on the front-end.

#####AngularJS and D3

CommonClarity is built as a single-page webapp using AngularJS for the front end. Flask is used to serve the initial template ([index.html](/templates/index.html)) and the other pages are in [partials](/static/partials). The routes and controllers are configured in [app.js](/static/js/app.js). The controllers in [controllers.js](/static/js/controllers.js) make calls to RESTful API endpoints and Angular directives are used to display the JSON.

The reports are drawn using D3 and a custom Angular directive [(directives.js)](/static/js/directives.js). Each directive contains a render and a watch function: to watch for changes to the data and re-render the graph when changes are detected. Each graph is scaled according to the amount of data returned and features legends and tooltips.

####Product Screenshots

[Click here to see a screenshot of the full dashboard](http://i.imgur.com/r8C9qb4.jpg), and scroll down for more product screenshots.

#####Student Set-up

Quickly set up your classes of students, either by uploading a CSV file or by manually entering each name.

![Student set-up](/static/screenshots/set_up_students.png =600x)

#####Easy Data Import

Easily import your standardized test data into the system.

![Data import](/static/screenshots/import_test_data.png =600x)

#####Reporting Dashboard

Once your data is imported, the reporting dashboard will automatically update with your new data.

[Click here for a full dashboard view.](http://i.imgur.com/r8C9qb4.jpg)
![Reporting dashboard](/static/screenshots/all_cohorts_dashboard.png)

Use the drill-down menus to view all your classes at once, one class individually, or one particular student.

![Drill-down](/static/screenshots/drill_down.png)

Target an individual student to see how that student is performing.

![Student dashboard](/static/screenshots/student_dashboard.png)

See how students are performing, according to specific Common Core State Standards.

![Standard report](/static/screenshots/standards_report.png)

Get a visual overview of how all the students in a class are performing.

![By student](/static/screenshots/class_perf_by_student.png)


####Try It Yourself!

1. Clone the repository:

    <code>$ git clone https://github.com/kkschick/CommonClarity.git</code>

2. Create and activate a new virtual environment:

    <code>$ virtualenv env</code>
    
    <code>$ . env/bin/activate/</code>
    
3. Install required packages:

    <code>$ pip install -r requirements.txt</code>

3. Run the app:

    <code>$ python app.py</code>

4. Point your browser to:

    <code>http://localhost:5000/</code>

5. Log into the app using kkschick@gmail.com / password to see a demo of the dashboard.
