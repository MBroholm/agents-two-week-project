# Example 02 — Combined vector and graph retrieval

## User question

Find the dream-stealing movie, then recommend two movies sharing its director.

## Tool calls and retrieved information

### 1. Movies — vector retrieval

**Execution:** 232 · **Status:** success

**Input**

```json
{
  "question": "movie about stealing dreams or dream-stealing"
}
```

**Output**

```json
{
  "executionId": "232",
  "status": "success",
  "data": {
    "Format results": [
      {
        "count": 5,
        "results": [
          {
            "imdbId": "tt1375666",
            "title": "Inception",
            "year": 2010,
            "plot": "Dom Cobb is a skilled thief, the absolute best in the dangerous art of extraction, stealing valuable secrets from deep within the subconscious during the dream state, when the mind is at its most vulnerable. Cobb's rare ability has made him a coveted player in this treacherous new world of corporate espionage, but it has also made him an international fugitive and cost him everything he has ever loved. Now Cobb is being offered a chance at redemption. One last job could give him his life back but only if he can accomplish the impossible, inception. Instead of the perfect heist, Cobb and his team of specialists have to pull off the reverse: their task is not to steal an idea, but to plant one. If they succeed, it could be the perfect crime. But no amount of careful planning or expertise can prepare the team for the dangerous enemy that seems to predict their every move. An enemy that only Cobb could have seen coming.",
            "source": "https://www.imdb.com/title/tt1375666/",
            "score": 0.8164456
          },
          {
            "imdbId": "tt0137523",
            "title": "Fight Club",
            "year": 1999,
            "plot": "A nameless first-person narrator attends support groups in an attempt to subdue his emotional state and relieve his insomniac state. When he meets Marla, another fake attendee of support groups, his life seems to become a little more bearable. However, when he associates himself with Tyler he is dragged into an underground fight club and soap-making scheme. Together the two men spiral out of control and engage in competitive rivalry for love and power.",
            "source": "https://www.imdb.com/title/tt0137523/",
            "score": 0.78556657
          },
          {
            "imdbId": "tt1130884",
            "title": "Shutter Island",
            "year": 2010,
            "plot": "In 1954, up-and-coming U.S. marshal Teddy Daniels is assigned to investigate the disappearance of a patient from Boston's Shutter Island Ashecliffe Hospital. He's been pushing for an assignment on the island for personal reasons, but before long he thinks he's been brought there as part of a twisted plot by hospital doctors whose radical treatments range from unethical to illegal to downright sinister. Teddy's shrewd investigating skills soon provide a promising lead, but the hospital refuses him access to records he suspects would break the case wide open. As a hurricane cuts off communication with the mainland, more dangerous criminals \"escape\" in the confusion, and the puzzling, improbable clues multiply, Teddy begins to doubt everything - his memory, his partner, even his own sanity.",
            "source": "https://www.imdb.com/title/tt1130884/",
            "score": 0.78291273
          },
          {
            "imdbId": "tt0209144",
            "title": "Memento",
            "year": 2001,
            "plot": "Memento chronicles two separate stories of Leonard, an ex-insurance investigator who can no longer build new memories, as he attempts to find the murderer of his wife, which is the last thing he remembers. One story line moves forward in time while the other tells the story backwards revealing more each time.",
            "source": "https://www.imdb.com/title/tt0209144/",
            "score": 0.7796565
          },
          {
            "imdbId": "tt0133093",
            "title": "The Matrix",
            "year": 1999,
            "plot": "Thomas A. Anderson is a man living two lives. By day he is an average computer programmer and by night a hacker known as Neo. Neo has always questioned his reality, but the truth is far beyond his imagination. Neo finds himself targeted by the police when he is contacted by Morpheus, a legendary computer hacker branded a terrorist by the government. As a rebel against the machines, Neo must confront the agents: super-powerful computer programs devoted to stopping Neo and the entire human rebellion.",
            "source": "https://www.imdb.com/title/tt0133093/",
            "score": 0.7700548
          }
        ]
      }
    ]
  }
}
```

### 2. Movies — graph retrieval

**Execution:** 233 · **Status:** success

**Input**

```json
{
  "imdbId": "tt1375666"
}
```

**Output**

```json
{
  "executionId": "233",
  "status": "success",
  "data": {
    "Format results": [
      {
        "count": 5,
        "results": [
          {
            "selectedMovie": "Inception",
            "imdbId": "tt0209144",
            "title": "Memento",
            "year": 2001,
            "connection": "shared director",
            "person": "Christopher Nolan",
            "source": "https://www.imdb.com/title/tt0209144/"
          },
          {
            "selectedMovie": "Inception",
            "imdbId": "tt0816692",
            "title": "Interstellar",
            "year": 2014,
            "connection": "shared director",
            "person": "Christopher Nolan",
            "source": "https://www.imdb.com/title/tt0816692/"
          },
          {
            "selectedMovie": "Inception",
            "imdbId": "tt0482571",
            "title": "The Prestige",
            "year": 2006,
            "connection": "shared director",
            "person": "Christopher Nolan",
            "source": "https://www.imdb.com/title/tt0482571/"
          },
          {
            "selectedMovie": "Inception",
            "imdbId": "tt0468569",
            "title": "The Dark Knight",
            "year": 2008,
            "connection": "shared director",
            "person": "Christopher Nolan",
            "source": "https://www.imdb.com/title/tt0468569/"
          },
          {
            "selectedMovie": "Inception",
            "imdbId": "tt1130884",
            "title": "Shutter Island",
            "year": 2010,
            "connection": "shared actor",
            "person": "Leonardo DiCaprio",
            "source": "https://www.imdb.com/title/tt1130884/"
          }
        ]
      }
    ]
  }
}
```

## Agent’s actual answer

The dream-stealing movie from our imported collection is **Inception** (2010), directed by Christopher Nolan, where Dom Cobb steals valuable secrets from deep within the subconscious during the dream state.

Here are two movies sharing its director, Christopher Nolan:

1. **Memento** (2001) – Chronicles an ex-insurance investigator who can no longer build new memories as he attempts to find his wife's murderer. [IMDb reference](https://www.imdb.com/title/tt0209144/)
2. **Interstellar** (2014) – [IMDb reference](https://www.imdb.com/title/tt0816692/)
