# dbt Cloud IDE
A quick tour of the Cloud IDE. The IDE can be found by choosing the hamburger menu in the top left and selecting "Develop."

![pic](http://www.welcome.paprika.tech/dbtpic//dbtcloudide.png)

## Git controls
- All git commands in the IDE are completed here.
- This will change dynamically based on the git status of your project.
## File tree
- This is the main view into your dbt project.
- This is where a dbt project is built in the form of .sql, .yml, and other file types.
## Text editor
- This is where individual files are edited. You can open files by selecting them from the file tree to the left.
- You can open multiple files in tabs so you can easily switch between files.
- Statement tabs allow you to run SQL against your data warehouse while you are developing, but they are not saved in your project. If you want to save SQL queries, you can create .sql files in the analysis folder.
## Preview / Compile SQL
- These two buttons apply to statements and SQL files.
- Preview will compile and run your query against the data warehouse. The results will be displayed in the "Query Results" tab along the bottom of your screen.
- Compile SQL will compile any Jinja into pure SQL. This will be displayed in the Info Window in the "Compiled SQL" tab along the bottom of your screen.
## Info window
- This window will show results when you click on Preview or Compile SQL.
- This is helpful for troubleshooting errors during development.
- The "Lineage" will also show a diagram of the model that is currently open in the text editor and its ancestors and dependencies.
## Command line
- This is where you can execute specific dbt commands (e.g. dbt run, dbt test).
- This will pop up to show the results as they are processed. Logs can also be viewed here.
## View docs
- This button will display the documentation for your dbt project.
More details on this in the documentation module.
