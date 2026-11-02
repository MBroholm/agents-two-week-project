# Evaluation of example answers

**Agent model:** Gemini 3.5 Flash Lite.

The three files below contain each user question, named retrieval tool calls with inputs and outputs, and the agent's actual answer. The filenames describe their purpose clearly and are retained. This evaluation covers only these examples.

## 1. Semantic retrieval

**Question:** Which movie involves an astronaut stranded on Mars?

**Evidence:** [01-semantic-retrieval.md](01-semantic-retrieval.md) — execution 231.

**Retrieved information and result:** The agent called vector retrieval with `astronaut stranded on Mars`. The Martian ranked first (score 0.8390616), with its year, plot, and source. The agent identified The Martian (2015), summarised Watney's survival situation, and linked to IMDb.

**Assessment:** Successful. The answer is supported by the retrieved description and appropriately limits its scope to the imported collection. The similarity score is a ranking signal, not a confidence percentage.

## 2. Combined vector and graph retrieval

**Question:** Find the dream-stealing movie, then recommend two movies sharing its director.

**Evidence:** [02-combined-retrieval.md](02-combined-retrieval.md) — executions 232–233.

**Retrieved information and result:** Vector retrieval identified Inception and returned its IMDb ID. Graph retrieval used that ID to find movies connected through Christopher Nolan, as well as a separate shared-actor connection. The agent correctly selected Memento and Interstellar as shared-director recommendations and provided their source links.

**Assessment:** Successful. Both recommendations satisfy the requested relationship. Memento's description was already present in the vector results; Interstellar was listed without adding an unsupported plot summary. This demonstrates semantic identification followed by traversal of explicit graph relationships. A minor presentation limitation is that the final answer does not link Inception itself, although its source appears in the tool output.

## 3. Shared actor and thematic contrast

**Question:** Which movies share an actor with Inception but have completely different themes?

**Evidence:** [03-shared-actor-and-themes.md](03-shared-actor-and-themes.md) — executions 234–236.

**Retrieved information and result:** The agent searched for Inception, passed its IMDb ID to graph retrieval, and selected Shutter Island through the verified Leonardo DiCaprio connection. It then searched for Shutter Island to retrieve its plot before comparing the movies. It excluded director-only results and acknowledged limited OMDb cast coverage.

**Assessment:** Partially successful. The shared-actor relationship and most of the plot comparison are supported. However, the tools do not map actors to characters, so the claim that DiCaprio plays Dom Cobb and Teddy Daniels is unsupported by the retrieved evidence. Also, different premises do not establish *completely different themes*: both retrieved plots involve uncertainty about the mind or reality. The answer should qualify Shutter Island as a partial thematic contrast rather than imply full satisfaction of that requirement.

**Limitation demonstrated:** Even with correct retrieval, the agent can add facts absent from the tool results and overstate a subjective comparison. The small collection and incomplete listed casts also limit which connections can be found.
