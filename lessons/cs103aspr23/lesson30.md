# Aggregation and PA04

## Future Homeworks
PA03 Python SQL Transactions App was due Sunday 3/26
CA01 GPT Flask App was due Sunday 4/2 (group project)
PA04 NodeJS/Express Mongoose Transaction App due 4/16 (group or individual, your choice)
CA02 GPT NodeJS/Express/Mongoose App due Sunday 4/23 (individual project must store info in DB and share it)
PA95 React App due Sunday 4/30 (group or individual project, your choice)

## Activity -- Intro to Aggregation
Let's use the L37-start branch of the cs103aExpressApp which has examples of aggregation and which can be pushed to heroku.

We show how to do groupby and pivot tables using mongoose via these slides:
https://docs.google.com/presentation/d/1x8fbBwss3XEG3FOs1aTz8-WhIsgmBd1xFL60K9DMxn0/edit?usp=sharing


You may also want to look over this tutorial:
https://www.mongodb.com/docs/manual/reference/operator/aggregation/group/



## Useful links:
Aggregation Quick Reference:
https://www.mongodb.com/docs/manual/meta/aggregation-quick-reference/

### Particular Stages:
$project  produces new stream of documents with only the specified fields (see also $addfields)

$group  produces a new stream of document which contain a summary of the original stream
   this requires you to specify the fields to be grouped by (using the _id field)
   and the accumulation operator with an expression to accumulate

$match produces a new stream by filtering the current stream in the same way the Model.find operator works

$sort reorders the stream based on a list of fields that are specified to increase or decrease

