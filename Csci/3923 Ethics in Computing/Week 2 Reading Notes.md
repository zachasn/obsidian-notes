
#### A jargon-free explanation of how AI large language models work
- LLMs use a long list of numbers called *word vectors* to represent words. They do this because it's usefully for reasoning about spatial relationships. each word vector represents a point in vector space, and words with similar meanings are placed closer together. LLMs also use vector arithmetics to reason about words. For example, if you take the word *biggest* and subtract *big*  and add *small*, the word closet to the resulting vector is *smallest*.
- **Example:** 
	- Washington, DC, is at [38.9, 77]
	- New York is at [40.7, 74]
	- London is at [51.5, 0.1]
	- Paris is at [48.9, -2.4]
- Each layer of an LLM is a a neural network architecture called a *transformer*.
- 

