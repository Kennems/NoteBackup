# GPT Prompt engineering 

想想你的未来，如果连这种人都不如你怎么生存下去？甚至是从来都不齿于一瞧的人都能这样来欺负你。聪明点吧！兄弟

![](https://cdn.jsdelivr.net/gh/kennems/blog-image/20231228215808.png)

## Strategy : Write clear instuctions

### Tactic : Include details in your query to get more relevant answers

example

| Who’s president? | Who was the president of Mexico in 2021, and how frequently are elections held? |
| ---------------- | ------------------------------------------------------------ |

### Tactic : Ask the model to adopt a persona

example 

#### SYSTEM

When I ask for help to write something, you will reply with a document that contains at least one joke or playful comment in every paragraph.

### Tactic : Use delimiters(分隔符) to clearly indicate distinct parts of the input 

SYSTEM

You will be provided with a pair of articles (delimited with XML tags) about the same topic. First summarize the arguments of each article. Then indicate which of them makes a better argument and explain why.

**USER**

<article> insert first article here </article> <article> insert second article here </article>

### Tactic : Specify the steps required to complete a task 

SYSTEM

Use the following step-by-step instructions to respond to user inputs. Step 1 - The user will provide you with text in triple quotes. Summarize this text in one sentence with a prefix that says "Summary: ". Step 2 - Translate the summary from Step 1 into Spanish, with a prefix that says "Translation: ".

USER

"""insert text here"""

### Tactic : Provide examples

### Tactic : Specify the desired length of the output

Summarize the text delimited by triple quotes in about 50 words. """insert text here"""

### Tactic : Instruct the model to answer using a reference text

SYSTEM

Use the provided articles delimited by triple quotes to answer questions. If the answer cannot be found in the articles, write "I could not find an answer."

USER

<insert articles, each delimited by triple quotes> Question: <insert question here>

### Tactic : Instruct the model to answer with citations from a reference text 

SYSTEM

You will be provided with a document delimited by triple quotes and a question. Your task is to answer the question using only the provided document and to cite the passage(s) of the document used to answer the question. If the document does not contain the information needed to answer this question then simply write: "Insufficient information." If an answer to the question is provided, it must be annotated with a citation. Use the following format for to cite relevant passages ({"citation": …}).

USER

"""<insert document here>""" Question: <insert question here>

## Strategy : Split complex tasks into simpler subtasks

### Tactic : Use intent classification to identify the most relevant instructions for a user query 

### Tactic : For dialogue applications that require very long conversations, summarize or filter previous dialogue

## Strategy : Give models time to "think"

### Tactic : Instruct the model to work out its own solution before rushing to a conclusion

### Tactic : Use inner monologue or a sequence of queries to hide the model's reasoning process

Follow these steps to answer the user queries. Step 1 - First work out your own solution to the problem. Don't rely on the student's solution since it may be incorrect. Enclose all your work for this step within triple quotes ("""). Step 2 - Compare your solution to the student's solution and evaluate if the student's solution is correct or not. Enclose all your work for this step within triple quotes ("""). Step 3 - If the student made a mistake, determine what hint you could give the student without giving away the answer. Enclose all your work for this step within triple quotes ("""). Step 4 - If the student made a mistake, provide the hint from the previous step to the student (outside of triple quotes). Instead of writing "Step 4 - ..." write "Hint:".

### Tactic : Ask the model if it missed anything on previous passes



## Strategy : Test changes systematically

### Tactic : Evaluate model outputs with reference to gold-standard answers

