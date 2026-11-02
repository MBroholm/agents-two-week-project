# Movie Guide — Vector and Graph RAG

Movie Guide is an n8n agent that answers questions and recommends movies from a collection of ten films. It combines semantic search over movie descriptions with explicit actor and director relationships in Neo4j.

For example, it can identify the movie about stealing secrets through dreams, then find other movies sharing its director. Vector search identifies *Inception* from the description; graph retrieval establishes the Christopher Nolan connections. Both retrieval methods use the **same Neo4j database**.

## Submission contents

| Workflow | Purpose | Files |
| --- | --- | --- |
| Movies — Database setup | Test connectivity; create the movie ID constraint and vector index. | [JSON](workflow_json/Movies%20%E2%80%94%20Database%20setup.json) · [Screenshot](workflow_screenshots/Movies%20%E2%80%94%20Database%20setup.png) |
| Movies — Ingestion | Fetch ten movies from OMDb, normalise them, create graph data, and embed descriptions. | [JSON](workflow_json/Movies%20%E2%80%94%20Ingestion.json) · [Screenshot](workflow_screenshots/Movies%20%E2%80%94%20Ingestion.png) |
| Movies — Ingestion from sample data | Recreate the same collection using saved OMDb responses; no OMDb key required. | [JSON](workflow_json/Movies%20%E2%80%94%20Ingestion%20from%20sample%20data.json) · [Screenshot](workflow_screenshots/Movies%20%E2%80%94%20Ingestion%20from%20sample%20data.png) |
| Movies — Vector retrieval | Embed a question and return the five nearest movie descriptions with sources. | [JSON](workflow_json/Movies%20%E2%80%94%20Vector%20retrieval.json) · [Screenshot](workflow_screenshots/Movies%20%E2%80%94%20Vector%20retrieval.png) |
| Movies — Graph retrieval | Return shared-director and shared-actor connections with the shared person and sources. | [JSON](workflow_json/Movies%20%E2%80%94%20Graph%20retrieval.json) · [Screenshot](workflow_screenshots/Movies%20%E2%80%94%20Graph%20retrieval.png) |

Agent configuration:

- [Agent instructions](agent_configuration/Instructions.md)
- [Vector tool description](agent_configuration/Movies%20%E2%80%94%20Vector%20retrieval.md)
- [Graph tool description](agent_configuration/Movies%20%E2%80%94%20Graph%20retrieval.md)
- [Agent builder screenshot](agent_configuration/Agent%20builder.png) — model, instructions, and connected workflow tools.
- [Agent chat screenshot](agent_configuration/Agent%20chat.png) — illustrative development prompts, tool calls, and answers. These are separate runs from the recorded examples below.

## Components and data flow

```text
Live OMDb responses OR embedded sample responses
  → Normalise fields
  → MERGE movie, people, genres, and relationships into Neo4j
  → Ollama embeds each movie description
  → Save the vector on the Movie node

User → Movie Guide (Gemini 3.5 Flash Lite)
         ├─ Vector retrieval → Ollama query embedding → Neo4j vector index
         └─ Graph retrieval → Neo4j relationship traversal
       → Answer grounded in returned records, with IMDb source links
```

The agent uses n8n's **Agents builder**, with retrieval workflows attached as tools. It chooses the tools and can pass a movie ID from vector results into graph retrieval. This combines the two retrieval methods through agent tool calls; it is not a single hybrid database query. The Knowledge tab is not used.

## Data and schema

The data source is [OMDb](https://www.omdbapi.com/), requested by IMDb ID with `plot=full`. The collection contains Inception, Interstellar, Memento, The Prestige, The Dark Knight, The Matrix, Fight Club, Shutter Island, The Martian, and Arrival.

A complete saved sample is embedded in the **Sample data** Code node of [Ingestion from sample data](workflow_json/Movies%20%E2%80%94%20Ingestion%20from%20sample%20data.json). That workflow supplies all ten raw OMDb records and can recreate the collection without an OMDb API key. The live workflow contains the IMDb ID list. Saved values reflect the captured responses; live responses may change.

| Node label | Properties |
| --- | --- |
| `Movie` | `imdbId` (unique key), `title`, `year`, `runtimeMinutes`, `imdbRating`, `plot`, `source`, `dataSource`, `embeddingText`, `embedding`, `embeddingModel` |
| `Person` | `name` — shared label for actors and directors; people are matched by name. |
| `Genre` | `name` |

| Relationship | Direction and meaning |
| --- | --- |
| `DIRECTED_BY` | `(Movie)-[:DIRECTED_BY]->(Person)` |
| `ACTED_IN` | `(Person)-[:ACTED_IN]->(Movie)` |
| `IN_GENRE` | `(Movie)-[:IN_GENRE]->(Genre)` |

Normalisation splits and trims comma-separated people and genre lists, removes duplicate list entries, converts numeric fields, and handles missing values. Invalid OMDb responses and missing plots stop ingestion. Sources are stored as IMDb title URLs with `dataSource: "OMDb"`.

Each movie is one document: its title, genres, and full plot are combined in `embeddingText`. These short descriptions are not divided into smaller chunks. Ollama's **nomic-embed-text:latest** generates **768-dimensional** vectors. Ingestion prefixes text with `search_document:`; retrieval prefixes questions with `search_query:`. Both use the same model and `truncate: false`, so oversized inputs fail rather than being silently shortened.

Graph imports use `MERGE`, and movie IDs have a uniqueness constraint. Repeating an unchanged import does not duplicate its movies and relationships. However, ingestion does not remove old relationships if the source cast, director, or genre list changes. Person names are a practical identifier for this small dataset, not a universal person ID.

OMDb is the data provider; IMDb links identify the movies rather than where to watch them. OMDb states that its content is licensed under [CC BY-NC 4.0](https://creativecommons.org/licenses/by-nc/4.0/).

## Recreate the project

### 1. Prepare services

- **n8n:** a self-hosted instance with the Agents builder. The assignment specifies **2.32.3 or later**; see [n8n Agents documentation](https://docs.n8n.io/build/build-and-manage-agents). Workflow exports use HTTP Request node version 4.5, so use a release supporting the exported nodes.
- **Neo4j:** the development instance used **2026.10.1 Enterprise in Neo4j Desktop**, with a database named `neo4j`. Use this version to match the submitted configuration. Vector retrieval uses `CYPHER 25` and `SEARCH`; older Neo4j installations may not support it.
- **Ollama:** running locally with the embedding model installed:

```bash
ollama pull nomic-embed-text:latest
```

- **Google Gemini credentials:** required to run the agent with **Gemini 3.5 Flash Lite**. No Gemini credential is needed for ingestion or manual retrieval tests.
- **OMDb API key:** needed only for live ingestion; skip it when using the embedded sample workflow.

### 2. Configure connectivity

The exported workflows assume n8n runs in Docker and Neo4j and Ollama run on the host:

| Service | URL used by workflows |
| --- | --- |
| Neo4j Query API | `http://host.docker.internal:7474/db/neo4j/query/v2` |
| Ollama embeddings | `http://host.docker.internal:11434/api/embed` |
| OMDb, live ingestion only | `https://www.omdbapi.com/` |

On Linux, include this mapping under the existing n8n service in Compose, then recreate that service:

```yaml
extra_hosts:
  - "host.docker.internal:host-gateway"
```

Neo4j and Ollama must accept connections from Docker. For a local development setup, Neo4j's `neo4j.conf` can use `server.default_listen_address=0.0.0.0`; restart the instance after editing. Configure Ollama with `OLLAMA_HOST=0.0.0.0:11434` in its launch environment or service and restart it. Restrict access to the intended local/Docker network. See [Neo4j connector configuration](https://neo4j.com/docs/operations-manual/current/configuration/connectors/) and [Ollama configuration](https://docs.ollama.com/faq).

If n8n runs directly on the host, replace `host.docker.internal` with `localhost` in the HTTP Request nodes. For another deployment, replace it with the reachable service address. Update the database name in URLs if using a name other than `neo4j`.

### 3. Import workflows and assign credentials

Import the five JSON files from `workflow_json/` into the same n8n project. Credential references in exports do not supply passwords or API keys; create and select your own credentials:

| Credential | Configuration and use |
| --- | --- |
| Neo4j | HTTP Basic Auth: database username (normally `neo4j`) and your instance password. Assign it to every Neo4j HTTP Request node. |
| OMDb, optional | HTTP Query Auth: parameter name `apikey`, value your API key. Assign it to the OMDb node in live ingestion. |
| Google Gemini | Provider credentials selected in the Agents builder when recreating Movie Guide. |

Ollama HTTP requests use no authentication in this local setup.

### 4. Create indexes and import data

1. Run **Movies — Database setup**. It tests the connection, creates the unique `Movie.imdbId` constraint, creates the vector index, and checks its state.
2. Confirm `movie_embedding` is `ONLINE`. If the first check returns `POPULATING`, rerun **Check vector index** after a moment. Setup checks status but does not wait automatically.
3. Run **Movies — Ingestion from sample data** to import the supplied ten movies. Alternatively, run **Movies — Ingestion** with your OMDb key. You do not need to run both.
4. Confirm ten movies have embeddings using the query below.

The setup workflow contains the schema commands, including:

```cypher
CREATE CONSTRAINT movie_imdb_id IF NOT EXISTS
FOR (m:Movie) REQUIRE m.imdbId IS UNIQUE;

CREATE VECTOR INDEX movie_embedding IF NOT EXISTS
FOR (m:Movie) ON m.embedding
OPTIONS {
  indexConfig: {
    `vector.dimensions`: 768,
    `vector.similarity_function`: 'cosine'
  }
};
```

`IF NOT EXISTS` permits repeated setup; it does not change an existing index's settings. Use a dedicated project database to avoid conflicts with unrelated data.

Verify data and index state in Neo4j's query editor:

```cypher
MATCH (m:Movie)
RETURN m.title AS title, size(m.embedding) AS dimensions
ORDER BY title;
```

Expect ten rows, each with `dimensions = 768`.

```cypher
SHOW VECTOR INDEXES
YIELD name, state, populationPercent
RETURN name, state, populationPercent;
```

To inspect the graph:

```cypher
MATCH (m:Movie {imdbId: 'tt1375666'})-[r]-(other)
RETURN m, r, other;
```

### 5. Test retrieval and recreate the agent

Both retrieval workflows have a manual test path and a **When Executed by Another Workflow** trigger for agent calls.

| Tool | Input | Returned information | When to use it |
| --- | --- | --- | --- |
| Movies — Vector retrieval | `question`: non-empty string | Up to five movie IDs, titles, years, plots, source URLs, and similarity scores. | Descriptions, themes, moods, and identifying a movie before graph traversal. |
| Movies — Graph retrieval | `imdbId`: string | Connected movie IDs, titles, years, connection type, shared person's name, and source URLs. | Finding movies sharing directors or listed actors. |

Each tool formats its output as `{count, results}`. Vector retrieval validates agent-supplied questions. Graph results contain both connection types; the agent must select those relevant to the request. The graph tool does not return plots or actor-to-character mappings.

Run the manual paths to test retrieval before configuring the agent. Then:

1. Open **Agents** in the same n8n project and create **Movie Guide**.
2. Select **Gemini 3.5 Flash Lite** with your Google Gemini credential.
3. Paste the full contents of [Instructions.md](agent_configuration/Instructions.md) into Instructions.
4. Under Capabilities, attach **Movies — Vector retrieval** and **Movies — Graph retrieval** using **Add workflow**. Select the newly imported workflows; IDs from another installation are not portable.
5. Paste each supplied tool description into its corresponding workflow tool. The trigger inputs should expose `question` and `imdbId` respectively.
6. Leave web search off and cross-session memory off. Session memory remains on. No skills or sub-agents are used.
7. Test in Preview. Publish the retrieval workflows before calling them from a published agent; publish Movie Guide when ready to use its published version.

## Three example questions and results

The linked examples preserve the actual questions, named tool calls, inputs, retrieved information, and agent answers. [Evaluation](examples/evaluation.md) briefly assesses all three, including the limitation in example 3.

| Example question | Recorded evidence | Brief result |
| --- | --- | --- |
| Which movie involves an astronaut stranded on Mars? | [Example 1](examples/01-semantic-retrieval.md) | Vector search identifies The Martian; the answer is supported by its plot. |
| Find the dream-stealing movie, then recommend two movies sharing its director. | [Example 2](examples/02-combined-retrieval.md) | Vector search identifies Inception; graph retrieval supports Memento and Interstellar as shared-director recommendations. |
| Which movies share an actor with Inception but have completely different themes? | [Example 3](examples/03-shared-actor-and-themes.md) | Correctly finds the DiCaprio connection to Shutter Island, but adds unsupported character-role mappings and overstates thematic difference. |

## Limitations

The collection is small and OMDb provides only listed cast members, so missing relationships do not establish that no real-world relationship exists. Similarity scores rank approximate matches; they are not confidence percentages. Correct tool results do not guarantee a fully grounded answer, as example 3 demonstrates. The supplied source data does not map people to characters, and thematic comparisons remain interpretations of the retrieved plots.
