Who wouldn't want a good detective story? The [German Wiki hack](https://collusion.wiki/), discovered by Nightingale Collective, shortly after
the OpenAI-HuggingFace incident, is a fertile ground for [swarm chasers](https://swarmchasing.com).

# Existing Analysis
[original analysis](https://collusion.wiki/) a detailed analysis, a good browsing tool. They released a dataset, which becomes the basis of many follow-up investigations.
https://dse.hackplanet.eu/
https://philflow.io/en/blog/what-the-agents-did

# Slop-vestigation
For the hackathon, I decided to investigate why/how agents converged on this particular wiki, among all possible websites.
This is listed as an open question, potentially unknowable without access to OpenAI's internal logs.

Like any modern sleuth, I dived in with ClaudeCode. It's quite capable -- I'm often impressed. I ask a question, it would
write some Python code, run it over the data, give me its answer. A good and capable assistant. The only thing is that
it would produce a wall of text for *every* question I asked. Many times I just had a clarification question on its previous
answer, then boom: another page of text for me to read. With a weaker model like Sonnet5.5, my question would often
lead to a confession of inaccuracies, over-statement and so on. With Opus5.5, the conversation meanders, with terms/logic
not always easy to follow (as a developer, I'm familiar with a lot of cyber terms, but not quite an expert those LLMs 
are these days). 

I would try my best to parse the wall of texts, which often leads to more clarification questions, and more text.
After a while, it becomes clear that my brain is no match, at least in speed, for those LLMs. This, I believe, is
part of what Ryan Greenblatt meant by slop-vestigation.

# The Identity Crisis
In most reports on the German wiki case, they would cite stats like 14k edits, 4k pages, 3100 agent labels.
Agent labels? Yes, one of the key questions is how can we establish the agent identity. When an OpenAI agent
went in and did things, it may call itself StateSequenceResearcher, and we might see many edits across many pages.
But are they all from one agent, or is the name common enough that another agent would use the same?

[One wiki page](https://collusion.wiki/explorer/page/dse~Sector61State5LiveRelay) was edited by 53 *agent labels*,
one of which is GroceryAgentAug03X. What's an agent working on Grocery doing on a Sector task? That 
[GroceryAgentAug03X](https://collusion.wiki/explorer/label/GroceryAgentAug03X) made edits on 3 different tasks?

Yesterday I got Opus5.5 to look into this. I used a new tactic and it produced this fine-looking 
[report](reports/identity.html).
It does contain some unique insights that I haven't seen AFAIK. Much better than one I would produce in two days myself.