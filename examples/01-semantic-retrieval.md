# Example 01 — Semantic retrieval

## User question

Which movie involves an astronaut stranded on Mars?

## Tool calls and retrieved information

### 1. Movies — vector retrieval

**Execution:** 231 · **Status:** success

**Input**

```json
{
  "question": "astronaut stranded on Mars"
}
```

**Output**

```json
{
  "executionId": "231",
  "status": "success",
  "data": {
    "Format results": [
      {
        "count": 5,
        "results": [
          {
            "imdbId": "tt3659388",
            "title": "The Martian",
            "year": 2015,
            "plot": "During a manned mission to Mars, Astronaut Mark Watney is presumed dead after a fierce storm and left behind by his crew. But Watney has survived and finds himself stranded and alone on the hostile planet. With only meager supplies, he must draw upon his ingenuity, wit and spirit to subsist and find a way to signal to Earth that he is alive. Millions of miles away, NASA and a team of international scientists work tirelessly to bring \"the Martian\" home, while his crewmates concurrently plot a daring, if not impossible, rescue mission. As these stories of incredible bravery unfold, the world comes together to root for Watney's safe return.",
            "source": "https://www.imdb.com/title/tt3659388/",
            "score": 0.8390616
          },
          {
            "imdbId": "tt0816692",
            "title": "Interstellar",
            "year": 2014,
            "plot": "In the near future around the American Midwest, Cooper, an ex-science engineer and pilot, is tied to his farming land with his daughter Murph and son Tom. As devastating sandstorms ravage Earth's crops, the people of Earth realize their life here is coming to an end as food begins to run out. Eventually stumbling upon a N.A.S.A. base 6 hours from Cooper's home, he is asked to go on a daring mission with a few other scientists into a wormhole because of Cooper's scientific intellect and ability to pilot aircraft unlike the other crew members. In order to find a new home while Earth decays, Cooper must decide to either stay, or risk never seeing his children again in order to save the human race by finding another habitable planet.",
            "source": "https://www.imdb.com/title/tt0816692/",
            "score": 0.7959287
          },
          {
            "imdbId": "tt1375666",
            "title": "Inception",
            "year": 2010,
            "plot": "Dom Cobb is a skilled thief, the absolute best in the dangerous art of extraction, stealing valuable secrets from deep within the subconscious during the dream state, when the mind is at its most vulnerable. Cobb's rare ability has made him a coveted player in this treacherous new world of corporate espionage, but it has also made him an international fugitive and cost him everything he has ever loved. Now Cobb is being offered a chance at redemption. One last job could give him his life back but only if he can accomplish the impossible, inception. Instead of the perfect heist, Cobb and his team of specialists have to pull off the reverse: their task is not to steal an idea, but to plant one. If they succeed, it could be the perfect crime. But no amount of careful planning or expertise can prepare the team for the dangerous enemy that seems to predict their every move. An enemy that only Cobb could have seen coming.",
            "source": "https://www.imdb.com/title/tt1375666/",
            "score": 0.7682043
          },
          {
            "imdbId": "tt0133093",
            "title": "The Matrix",
            "year": 1999,
            "plot": "Thomas A. Anderson is a man living two lives. By day he is an average computer programmer and by night a hacker known as Neo. Neo has always questioned his reality, but the truth is far beyond his imagination. Neo finds himself targeted by the police when he is contacted by Morpheus, a legendary computer hacker branded a terrorist by the government. As a rebel against the machines, Neo must confront the agents: super-powerful computer programs devoted to stopping Neo and the entire human rebellion.",
            "source": "https://www.imdb.com/title/tt0133093/",
            "score": 0.76654017
          },
          {
            "imdbId": "tt2543164",
            "title": "Arrival",
            "year": 2016,
            "plot": "Linguistics professor Louise Banks leads an elite team of investigators when gigantic spaceships touchdown in 12 locations around the world. As nations teeter on the verge of global war, Banks and her crew must race against time to find a way to communicate with the extraterrestrial visitors. Hoping to unravel the mystery, she takes a chance that could threaten her life and quite possibly all of mankind.",
            "source": "https://www.imdb.com/title/tt2543164/",
            "score": 0.762771
          }
        ]
      }
    ]
  }
}
```

## Agent’s actual answer

In our imported collection, the movie that involves an astronaut stranded on Mars is **The Martian** (2015). 

In the film, during a manned mission to Mars, astronaut Mark Watney is presumed dead after a fierce storm and left behind by his crew. Surviving the ordeal, he finds himself stranded and alone on the hostile planet, relying on his ingenuity and wit to subsist while NASA and a team of scientists work on a daring rescue mission.

- Source: [The Martian (IMDb reference)](https://www.imdb.com/title/tt3659388/)
