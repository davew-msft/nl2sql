# nl2sql @ DAMA Philly

Department of Behavioral Health and Intellectual disAbility Services
801 Market St
Philadelphia, PA 19107

9/10/2025
1-2pm 

## "Natural Language to SQL:  The Things You Might Not Have Thought About"

Being able to _chat with your data_ gets your users closer to the promise of true self-service analytics.  This is known as NL2SQL.  These solutions can be created in under an hour against just about any database.  However, just being able to have an LLM understand what you are asking and convert the request to SQL statements tends to be underwhelming.  What you need are some advanced techniques that most people don't think about that will make your solution truly valuable for your business users.  I'll show you how to embed business logic for YOUR business directly in the process.  Then I'll show you how to write QUALITY tests to ensure your users are getting the RIGHT answers from your data and we'll even show you how to have your LLM predict similar questions that your users may ask and offer those SUGGESTIONS to your users.  Then we'll show you how to ask PRESCRIPTIVE questions about your data that are nearly impossible to answer with just SQL, such as "Why did my sales go down last quarter and how can I prevent that next quarter?".  Finally, I'll show you how to build an MCP server that your users can leverage to run their queries.  

### About Dave Wentzel

* [dave@davewentzel.com](dave@davewentzel.com) 
* [davew@microsoft.com](davew@microsoft.com) 
* [Website](davewentzel.com) (I haven't posted much here for years)
* [LinkedIn](linkedin.com/in/dwentzel)

As [Microsoft Innovation Hub's](https://www.microsoft.com/en-us/hub) analytics architect, my mantra is "Business Before Technology".  At the Innovation Hub (nee Microsoft Technology Center/MTC) we strive to understand your business problems before deploying technology.  I focus on building solutions using data, analytics, and AI.  I work with our largest customers, from executives to business people to technologists, using a unique approach that helps to "shift left" analytics projects and remove risk.  The days of 6 month capital projects are over, instead I try to solve business problems quickly to prove value.  I try to build solutions that solve challenging problems prescriptively and repeatably.  SUCCESS for me is solving challenging problems for respected companies, working with talented people.  

## Agenda

* Intro:  Why are we here?  Why does this matter?  
* Quick overview of NL2SQL solutions using Microsoft Fabric (pre-baked, shows the _Art of the Possible_)
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

3. Get a SQL Server with AdventureWorks installed.  [Use the docker container with this repo](./adventureworks-sql-server-container/README.md)

