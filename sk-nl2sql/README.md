We will use Semantic Kernel to build out a full NL2SQL agentic solution.  



## Process

```bash
git clone https://https://github.com/davew-msft/semantic-kernel-advanced-usage

# open vscode to semantic-kernel-advanced-usage and start devcontainer when vscode prompts you
# in terminal (bash)
cd templates/natural_language_to_SQL/

pip install -r requirements.txt

#copy .env.sample to .env and set the vars
#stick with the models and api versions **whenever possible** listed in the env file.  There may be problems with SK not being 
#"aware" of newer API vers and different completions/gpt models

#let's just get this working with OOB sqllite db
python src/main.py "Give me the top 10 list of medications prescribed"

# use the UI
python src/server.py
#switch to browser and issue a similar query
#"Find doctors who have treated more than 5 patients"

#we now have the solution working with sqllite
```

## Get this working with SQL Server - Process

* Get a SQL Server instance running with your database and your connstring details.  
* I'm using AdvWorks running in a local docker container (I can provide instructions)

* edit db_helpers.py and change the vars around Line 67

```bash


```