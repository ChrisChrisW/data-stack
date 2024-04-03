
# Documenting your analytics

## What you will learn in this course? 🧐🧐
* What is documenting your analytics important
* How dbt helps you handle documentation


Let’s go back to the last time you were onboarded in an organization, how long did it take you to get wrapped, so you could start contributing to that organization in a meaningful way ?

Did this process involve a lot of slack or team messages, zoom calls, or a bunch of emails ? If not this was a pretty quick process - I would imagine that the documentation in your organization is in a really good spot and you can self-serve and get answers right away and start contributing as soon as possible.

If not documentation may be something to consider to improve how quickly people can get around and can start contributing.

In analytics, documentation is particularly important : we want users to be able to answer their own questions, like what does this data means ? How is it calculated ? Where is it coming from ?

The issue is that often coding and producing the data artifact is separated from  the actual documentation of those artifacts 

In dbt thought both are kept as close as possible by including documentation in yaml files in the project itself - alongside the models that you write.

This documentation can then be served in a very friendly user interface so that people can answer their own questions.


## Documentation in a nutshell

- Documentation is essential for an analytics team to work effectively and efficiently. Strong documentation empowers users to self-service questions about data and enables new team members to on-board quickly.
- Documentation often lags behind the code it is meant to describe. This can happen because documentation is a separate process from the coding itself that lives in another tool.
- Therefore, documentation should be as automated as possible and happen as close as possible to the coding.
- In dbt, models are built in SQL files. These models are documented in YML files that live in the same folder as the models.

## Writing documentation and doc blocks

- Documentation of models occurs in the YML files (where generic tests also live) inside the models directory. It is helpful to store the YML file in the same subfolder as the models you are documenting.
- For models, descriptions can happen at the model, source, or column level.
- If a longer form, more styled version of text would provide a strong description, **doc blocks** can be used to render markdown in the generated documentation.

## Generating and viewing documentation
- In the command line section, an updated version of documentation can be generated through the command dbt *docs generate*. This will refresh the 'view docs' link in the top left corner of the Cloud IDE.
- The generated documentation includes the following:
    - Lineage Graph
    - Model, source, and column descriptions
    - Generic tests added to a column
    - The underlying SQL code for each model
    - and more...


## Resources 📚📚

* [everything on documentation](https://docs.getdbt.com/docs/collaborate/documentation)



