# ASSIGNMENT: Sampling and Reproducibility in Python

Read the blog post [Contact tracing can give a biased sample of COVID-19 cases](https://andrewwhitby.com/2020/11/24/contact-tracing-biased/) by Andrew Whitby to understand the context and motivation behind the simulation model we will be examining.

Examine the code in `whitby_covid_tracing.py`. Identify all stages at which sampling is occurring in the model. Describe in words the sampling procedure, referencing the functions used, sample size, sampling frame, any underlying distributions involved, and how these relate to the procedure outlined in the blog post.

Run the Python script file called whitby_covid_tracing.py as is and compare the results to the graphs in the original blog post. Does this code appear to reproduce the graphs from the original blog post?

Modify the number of repetitions in the simulation to 1000 (from the original 50000). Run the script multiple times and observe the outputted graphs. Comment on the reproducibility of the results.

Alter the code so that it is reproducible. Describe the changes you made to the code and how they affected the reproducibility of the script file. The output does not need to match Whitby’s original blogpost/graphs, it just needs to produce the same output when run multiple times

# Author: Viktoriia Peleshko

```
Please write your explanation here...

1. I have read the blog post by Andrew Whitby and examined the code in `whitby_covid_tracing.py`. 

Sampling is ocuring at the next stages: 

- Sample of people to infect 
  infected_indices = np.random.choice(ppl.index, size=int(len(ppl) * ATTACK_RATE), replace=False)
  ppl.loc[infected_indices, 'infected'] = True
Sampling procedure - random sample of people indices. Sample size - ATTACK_RATE, equal for 10% of people randomly chosen. Sampling frame - 1000 people at the all events (wedding * 200 + brunch * 800). Underlying distributions - random distribution to selected individuals. Relate to the blog post - any person could be infected at the event. 

- Sample of primary contact tracing
  ppl.loc[ppl['infected'], 'traced'] = np.random.rand(sum(ppl['infected'])) < TRACE_SUCCESS
Sampling procedure - random sample of infected people for tracing. Sample size - all infected people. Sampling frame - people infected = True. Underlying distributions - if the random number is less then TRACE_SUCCESS (20%), the individuals are traced. Relate to the blog post - not all infected individuals will be traced.

- Sample of secondary contact tracing based on event attendance
  event_trace_counts = ppl[ppl['traced'] == True]['event'].value_counts()
  events_traced = event_trace_counts[event_trace_counts >= SECONDARY_TRACE_THRESHOLD].index
  ppl.loc[ppl['event'].isin(events_traced) & ppl['infected'], 'traced'] = True
Sampling procedure - secondary contact tracing, based on event attendance. Sample size - the number of people who will be traced depends on the number of traced individuals at each event type. Sampling frame - individuals who are traced and infected, and filtered by events. Underlying distributions - all infected individuals are tracing if >= SECONDARY_TRACE_THRESHOLD (2). Relate to the blog post - some events could have stronger affect to the tracing depends on their size and popularity. 

2. I have run the Python script. The outputted graph of script looks differently comparing with graph in the blog post. Graph from the script show some correlation between contact tracing and perceived infection sources. In the blog's graph we don't see this trend. 

3. Next, I have made the dublicate of this script modified the number of repetitions in the simulation to 1000 (from the original 50000). I have run this script a few times. In each case I received different result because the random seed was not applied. 

4. Then, I have made the script reproducible adding the random seed (42). It helps to get the same result for all random number generation. As the result, after each run I got the same graph. The output does not match Whitby’s blogpost graph. 


## Criteria

|Criteria|Complete|Incomplete|
|--------|----|----|
|Altercation of the code|The code changes made, made it reproducible.|The code is still not reproducible.|
|Description of changes|The author explained the reasonings for the changes made well.|The author did not explain the reasonings for the changes made well.|

## Submission Information

🚨 **Please review our [Assignment Submission Guide](https://github.com/UofT-DSI/onboarding/blob/main/onboarding_documents/submissions.md)** 🚨 for detailed instructions on how to format, branch, and submit your work. Following these guidelines is crucial for your submissions to be evaluated correctly.

### Submission Parameters:
* Submission Due Date: `HH:MM AM/PM - DD/MM/YYYY`
* The branch name for your repo should be: `sampling-and-reproducibility`
* What to submit for this assignment:
    * This markdown file (sampling_and_reproducibility.md) should be populated.
    * The `whitby_covid_tracing.py` should be changed.
* What the pull request link should look like for this assignment: `https://github.com/<your_github_username>/sampling/pull/<pr_id>`
    * Open a private window in your browser. Copy and paste the link to your pull request into the address bar. Make sure you can see your pull request properly. This helps the technical facilitator and learning support staff review your submission easily.

Checklist:
- [ ] Create a branch called `sampling-and-reproducibility`.
- [ ] Ensure that the repository is public.
- [ ] Review [the PR description guidelines](https://github.com/UofT-DSI/onboarding/blob/main/onboarding_documents/submissions.md#guidelines-for-pull-request-descriptions) and adhere to them.
- [ ] Verify that the link is accessible in a private browser window.

If you encounter any difficulties or have questions, please don't hesitate to reach out to our team via our Slack at `#cohort-3-help`. Our Technical Facilitators and Learning Support staff are here to help you navigate any challenges.
