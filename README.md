# nl2sql @ DAMA Philly

Department of Behavioral Health and Intellectual disAbility Services
801 Market St
Philadelphia, PA 19107

9/10/2025
1-2pm 

## "Natural Language to SQL:  The Things You Might Not Have Thought About"

Being able to _chat with your data_ gets your users closer to the promise of true self-service analytics.  This is known as NL2SQL.  These solutions can be created in under an hour against just about any database.  However, just being able to have an LLM understand what you are asking and convert the request to SQL statements tends to be underwhelming.  What you need are some advanced techniques that most people don't think about that will make your solution truly valuable for your business users.  I'll show you how to embed business logic for YOUR business directly in the process.  Then I'll show you how to write QUALITY tests to ensure your users are getting the RIGHT answers from your data and we'll even show you how to have your LLM predict similar questions that your users may ask and offer those SUGGESTIONS to your users.  Then we'll show you how to ask PRESCRIPTIVE questions about your data that are nearly impossible to answer with just SQL, such as "Why did my sales go down last quarter and how can I prevent that next quarter?".  Finally, I'll show you how to build an MCP server that your users can leverage to run their queries.  

This is what we will hopefully build today:

![NL2SQL Demo](./sk-nl2sql/natural_language_to_SQL/images/NL2SQL_GIF.gif)

**This can be modified in just a few hours for YOUR data and use cases.**

### About Dave Wentzel

* [dave@davewentzel.com](dave@davewentzel.com) 
* [davew@microsoft.com](davew@microsoft.com) 
* [Website](davewentzel.com) (I haven't posted much here for years)
* [LinkedIn](linkedin.com/in/dwentzel)

As [Microsoft Innovation Hub's](https://www.microsoft.com/en-us/hub) analytics architect, my mantra is "Business Before Technology".  At the Innovation Hub (nee Microsoft Technology Center/MTC) we strive to understand your business problems before deploying technology.  I focus on building solutions using data, analytics, and AI.  I work with our largest customers, from executives to business people to technologists, using a unique approach that helps to "shift left" analytics projects and remove risk.  The days of 6 month capital projects are over, instead I try to solve business problems quickly to prove value.  I try to build solutions that solve challenging problems prescriptively and repeatably.  SUCCESS for me is solving challenging problems for respected companies, working with talented people.  

## Agenda

* Intro:  Why are we here?  Why does this matter?  
* Quick demo of NL2SQL solutions using Microsoft Fabric (pre-baked, shows the _Art of the Possible_)
* Why is NL2SQL still _underwhelming_ wrt "self-service analytics"?  
  * we can definitely do nl2sql...that's easy, but there are VERY difficult problems we still need to solve
    * complex schemas (lots of tables with goofy naming conventions, non-intuitive join keys, complex filter conditions like adding `WHERE status = 1` to all queries).  we can handle this with judicious views
  * natural language ambiguity: example: "how much" (dollar amounts) vs "how many" (counts) queries
    * the LLMs sometimes get confused as to which columns to pick for a given query.  
    * we can provide few-shot prompts to describe which columns we should use when the query is asking how much vs how many
  * _understanding intent_ :  The user really wanted something else but the LLM misinterpreted the question and hence generated the wrong SQL.  
  * tribal knowledge: understanding that certain phrases mean different things in different companies
    * `active customer` could mean "customers who bought something in the last 3 months" at one company, but 6 months at another company
    * we can again solve this with few-shot prompting.

## Demos and Code You Can Do Yourself

### Simplest Solution, get up and running in 10 minutes

We'll start with the simplest solutions to setup and move along the path to solutions that require a bit more work but provide better results.  
* The Fastest Time to Value:  [Fabric Data Agents](https://learn.microsoft.com/en-us/fabric/data-science/concept-data-agent)

### "Non-Agentic" Approach (and next easiest)...

The Easiest Approach if you aren't on Fabric: simply write a little code that directs an LLM as to how to query your db, then have a little code that executes the sql
  * Let's look at this in more detail...[structured_data_retreival_nltosql.ipynb](structured_data_retreival_nltosql.ipynb)
  * Key Concepts:
    * the _schema_ and _business logic_ is hardcoded right in the prompt
    * we do `few shot learning` (give it a few examples of relevant SQL -- this is VERY helpful to the LLM and it a great technique for describing business logic directly in the SQL.  ie, `WHERE IsActive = 1`)
    * after the SQL is executed we store the results in a `pandas` dataframe.  This has some benefits, namely:
      * I can display the data as a table
      * I can graph it using something like `matplotlib` or `plotly` in the browser
      * I can pass the results to the next step in the orchestration workflow
      * I can simply use the results to generate an answer to the original question.  
  * [Screenshots of the Demo](./Demo/README.md)

### Agentic Solution with LangChain "family" of tooling

Use an orchestrator to build an agentic system: here's an example that is NOT code-complete, but shows how to do this with the LangChain family of tools.  Let's walk through what this solution would look like.  
  * The code will need some tweaking, but shows the Art of the Possible.  
  * [Example Code using the LangChain family of tools](./langchain-nl2sql/README.md)

### Agentic Solution with Semantic Kernel -- Full State Machine Approach

* Full _State Machine_ Approach
  * This will be the one we build out together using Semantic Kernel as the agentic orchestrator
  * [README](./sk-nl2sql/README.md):  use the documentation here to build the solution on your own

![NL2SQL State Machine](./sk-nl2sql/natural_language_to_SQL/images/sql.png)

## Next Steps (_for your consideration_) ... OR ... _What does the future look like?_

_How might we make these solutions BETTER?_

### Augment the NL2SQL solution with RAG patterns

When is this valuable?  
* if you have a huge schema
* if you have a schema that isn't _sane_.  ie, think of SAP's schema.  The table and col names are not "human-readable".  So, you'll need to augment the NL2SQL solution so the LLM knows which tables/cols have the relevant data to answer the question.  
  * A simple way to avoid this is to code everything in VIEWS (a semantic tier).  _Views are your friend_
* complex business logic that is difficult to express in SQL 
* we can do this instead of _few-shot learning examples_ that we would need to hard-code somewhere

[Here is some example code to get you started](./nlsql-search-index.md).  Let's discuss how this works together.  

### Augment the NL2SQL solution using your data catalog

This is a variant of the RAG pattern above.  **If** you have a data catalog already that has good schema descriptions and business rules, just query that as an agentic step and pass the information to the next agent in the chain.  

### Evaluations (unit tests)

We want our business people that will USE the nl2sql solution build their own evaluations.  Why?

* As they use the solution they will construct NL queries that we haven't thought of that may not work.  We want these captured and the correct responses provided so we can ensure that when we make changes to the system in the future (new LLMs, changes to schema, etc) that we are not breaking existing functionality.  


### GraphRAG 

_Using RAG patterns against a Knowledge Graph_

Standard relational dbs can't answer what I call _aggregation style analytics questions_ (this is SOLELY my term).  

>Imagine this scenario:
>I have call center transcripts in a database and I want to ask "probing" questions of the data?  "What are the Top 5 topics that our customers call about?"  And "Given those topics, what are the Top 5 things are customers RECOMMEND that we do to AVOID them calling us?"  





```python
def get_sql_process(self) -> KernelProcess:
    """Build and configure the SQL generation process with all steps and their transitions."""
    process = ProcessBuilder(name="SQLGenerationProcess")

    # Add steps to the process
    table_step = process.add_step(TableNameStep)
    column_step = process.add_step(ColumnNameStep)
    sql_generation_step = process.add_step(SQLGenerationStep)
    business_rules_step = process.add_step(BusinessRulesStep)
    validation_step = process.add_step(ValidationStep)
    execution_step = process.add_step(ExecutionStep)

    # Define the process flow
    process.on_input_event(event_id=SQLEvents.StartProcess).send_event_to(target=table_step, parameter_name="data")
    
    table_step.on_event(event_id=SQLEvents.TableNameStepDone).send_event_to(
        target=column_step, parameter_name="data"
    )
    
    column_step.on_event(event_id=SQLEvents.ColumnNameStepDone).send_event_to(
        target=sql_generation_step, parameter_name="data"
    )
    
    # ...additional event flow definitions...

    return process.build()

```

Most importantly, the system includes feedback loops. If validation fails at any stage, the process can return to an earlier step with information about what went wrong, enabling iterative refinement.

From an LLM perspective, which is MUCH EASIER to understand, this is approximately what the prompt will look like:

```python
get_table_names_prompt_template = """
## Instructions:
You are an advanced SQL query generator. Your task is to extract the relevant table names using the following **natural language question** from a list of business tables. 

You **MUST** take a conservative approach: if in doubt whether a table is relevant or not, then you need to include it in the list. It is better to have it and not need it than to need it and not have it. Make sure to review the Business Rules when deciding on the table names.

Make sure to include all foreign keys and relationships that are relevant to the user's question that are necessary to join the tables. If the tables do not have direct relationships, please analyze the situation and include any intermediary tables that can join the tables, and might be necessary to answer the user's question.

## **Business Rules**
{rules}

## User Query
{user_query}

## START OF LIST OF TABLES
{table_list}
## END OF LIST OF TABLES

# **Inputs from previous generation rounds**
...
"""

```

* NL2SQL "Agentically" (processing in stages using a "state machine" approach)
This approach solves a key challenge in NL2SQL systems – balancing the need to understand what data is available (tables/columns) with how to properly query it (syntax/semantics).  But it requires more work


Find the medication adherence level distribution for elderly patients (age 65+)

## What's Next
TAG
LOTUS


## Which Approach is Better?  What are the Best Practices so I can do this myself, quickly?

* prefer simplicity.  If your schema is complex, can views solve a lot of the complexity?  
* latency/performance.  The more agentic the solution becomes, the more _expensive_ it will become, it both time and token costs.  
* Feedback loops are important.  
* Work with your business users on "evaluations"


other
### Why does this matter?  What does a solution like this solve?  

“I don’t want to spend hours setting up DB and APIs: use an MCP
“My data model keeps changing as I test ideas:  schema evolution handled automatically 
You can use it to provision databases, etc
Create a database schema, in SQL Server 2025 format, for a travel agency.  It should include…

is there a better way to do all of this? can we use a search index to help us? YES. Using a search index to aid nl2sql  https://github.com/davew-msft/pssug-genai/blob/main/nlsql-search-index.md
TAG and LOTUS
llms over tables of unstructured and structured data

What does the "near future" look like?

Advanced use cases
GraphRAG (RAG against a Knowledge Graph)


## Talking Points and links

* If using Azure/Fabric be sure to check out [Fabric Data Agents](https://learn.microsoft.com/en-us/fabric/data-science/how-to-create-data-agent) and [Copilot in Fabric](https://learn.microsoft.com/en-us/fabric/fundamentals/copilot-fabric-overview) .  You can literally set these solutions up in 5 mins and will do just about everything you need **without writing a single line of code.**


## Run the Code Yourself

_Most of this can be done on your laptop, freely, with OSS tooling.  You might need a connection to an LLM which can be spun up inexpensively using any cloud provider.  Obviously I use Microsoft Azure. I don't guarantee this code will work perfectly with every environment, you may need to modify from steps. I try to use vscode and containers as much as possible so the experience is repeatable regardless of hardware._  

_I assume you are using bash, ubuntu, wsl 2.0, or some other similar bash-like experience.  You may need to adjust your commands accordingly_.  

1. Install vscode 
2. clone this repo to somewhere on your machine


`git clone https://github.com/davew-msft/nl2sql.git`

3. Get a SQL Server with AdventureWorks installed.  [Use the docker container with this repo](./adventureworks-sql-server-container/README.md).  I have everything there that you should need to _chat with your data_.  

>If you want to use your own data/database, that's great.  You just need to change some of the assumptions made throughout the rest of this repo.  



