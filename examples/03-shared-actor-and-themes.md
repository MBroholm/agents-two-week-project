# Example 03 — Shared actor and thematic contrast

## User question

Which movies share an actor with Inception but have completely different themes?

## Tool calls and retrieved information

### 1. Movies — vector retrieval

**Execution:** 234 · **Status:** success

**Input**

```json
{
  "question": "Inception"
}
```

**Output**

```json
{
  "executionId": "234",
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
            "score": 0.8978245
          },
          {
            "imdbId": "tt2543164",
            "title": "Arrival",
            "year": 2016,
            "plot": "Linguistics professor Louise Banks leads an elite team of investigators when gigantic spaceships touchdown in 12 locations around the world. As nations teeter on the verge of global war, Banks and her crew must race against time to find a way to communicate with the extraterrestrial visitors. Hoping to unravel the mystery, she takes a chance that could threaten her life and quite possibly all of mankind.",
            "source": "https://www.imdb.com/title/tt2543164/",
            "score": 0.79280806
          },
          {
            "imdbId": "tt1130884",
            "title": "Shutter Island",
            "year": 2010,
            "plot": "In 1954, up-and-coming U.S. marshal Teddy Daniels is assigned to investigate the disappearance of a patient from Boston's Shutter Island Ashecliffe Hospital. He's been pushing for an assignment on the island for personal reasons, but before long he thinks he's been brought there as part of a twisted plot by hospital doctors whose radical treatments range from unethical to illegal to downright sinister. Teddy's shrewd investigating skills soon provide a promising lead, but the hospital refuses him access to records he suspects would break the case wide open. As a hurricane cuts off communication with the mainland, more dangerous criminals \"escape\" in the confusion, and the puzzling, improbable clues multiply, Teddy begins to doubt everything - his memory, his partner, even his own sanity.",
            "source": "https://www.imdb.com/title/tt1130884/",
            "score": 0.7770169
          },
          {
            "imdbId": "tt0816692",
            "title": "Interstellar",
            "year": 2014,
            "plot": "In the near future around the American Midwest, Cooper, an ex-science engineer and pilot, is tied to his farming land with his daughter Murph and son Tom. As devastating sandstorms ravage Earth's crops, the people of Earth realize their life here is coming to an end as food begins to run out. Eventually stumbling upon a N.A.S.A. base 6 hours from Cooper's home, he is asked to go on a daring mission with a few other scientists into a wormhole because of Cooper's scientific intellect and ability to pilot aircraft unlike the other crew members. In order to find a new home while Earth decays, Cooper must decide to either stay, or risk never seeing his children again in order to save the human race by finding another habitable planet.",
            "source": "https://www.imdb.com/title/tt0816692/",
            "score": 0.7754425
          },
          {
            "imdbId": "tt0468569",
            "title": "The Dark Knight",
            "year": 2008,
            "plot": "Set within a year after the events of Batman Begins (2005), Batman, Lieutenant James Gordon, and new District Attorney Harvey Dent successfully begin to round up the criminals that plague Gotham City, until a mysterious and sadistic criminal mastermind known only as \"The Joker\" appears in Gotham, creating a new wave of chaos. Batman's struggle against The Joker becomes deeply personal, forcing him to \"confront everything he believes\" and improve his technology to stop him. A love triangle develops between Bruce Wayne, Dent, and Rachel Dawes.",
            "source": "https://www.imdb.com/title/tt0468569/",
            "score": 0.76848555
          }
        ]
      }
    ]
  }
}
```

### 2. Movies — graph retrieval

**Execution:** 235 · **Status:** success

**Input**

```json
{
  "imdbId": "tt1375666"
}
```

**Output**

```json
{
  "executionId": "235",
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

### 3. Movies — vector retrieval

**Execution:** 236 · **Status:** success

**Input**

```json
{
  "question": "Shutter Island"
}
```

**Output**

```json
{
  "executionId": "236",
  "status": "success",
  "data": {
    "Format results": [
      {
        "count": 5,
        "results": [
          {
            "imdbId": "tt1130884",
            "title": "Shutter Island",
            "year": 2010,
            "plot": "In 1954, up-and-coming U.S. marshal Teddy Daniels is assigned to investigate the disappearance of a patient from Boston's Shutter Island Ashecliffe Hospital. He's been pushing for an assignment on the island for personal reasons, but before long he thinks he's been brought there as part of a twisted plot by hospital doctors whose radical treatments range from unethical to illegal to downright sinister. Teddy's shrewd investigating skills soon provide a promising lead, but the hospital refuses him access to records he suspects would break the case wide open. As a hurricane cuts off communication with the mainland, more dangerous criminals \"escape\" in the confusion, and the puzzling, improbable clues multiply, Teddy begins to doubt everything - his memory, his partner, even his own sanity.",
            "source": "https://www.imdb.com/title/tt1130884/",
            "score": 0.88085
          },
          {
            "imdbId": "tt1375666",
            "title": "Inception",
            "year": 2010,
            "plot": "Dom Cobb is a skilled thief, the absolute best in the dangerous art of extraction, stealing valuable secrets from deep within the subconscious during the dream state, when the mind is at its most vulnerable. Cobb's rare ability has made him a coveted player in this treacherous new world of corporate espionage, but it has also made him an international fugitive and cost him everything he has ever loved. Now Cobb is being offered a chance at redemption. One last job could give him his life back but only if he can accomplish the impossible, inception. Instead of the perfect heist, Cobb and his team of specialists have to pull off the reverse: their task is not to steal an idea, but to plant one. If they succeed, it could be the perfect crime. But no amount of careful planning or expertise can prepare the team for the dangerous enemy that seems to predict their every move. An enemy that only Cobb could have seen coming.",
            "source": "https://www.imdb.com/title/tt1375666/",
            "score": 0.744636
          },
          {
            "imdbId": "tt0209144",
            "title": "Memento",
            "year": 2001,
            "plot": "Memento chronicles two separate stories of Leonard, an ex-insurance investigator who can no longer build new memories, as he attempts to find the murderer of his wife, which is the last thing he remembers. One story line moves forward in time while the other tells the story backwards revealing more each time.",
            "source": "https://www.imdb.com/title/tt0209144/",
            "score": 0.7335255
          },
          {
            "imdbId": "tt0816692",
            "title": "Interstellar",
            "year": 2014,
            "plot": "In the near future around the American Midwest, Cooper, an ex-science engineer and pilot, is tied to his farming land with his daughter Murph and son Tom. As devastating sandstorms ravage Earth's crops, the people of Earth realize their life here is coming to an end as food begins to run out. Eventually stumbling upon a N.A.S.A. base 6 hours from Cooper's home, he is asked to go on a daring mission with a few other scientists into a wormhole because of Cooper's scientific intellect and ability to pilot aircraft unlike the other crew members. In order to find a new home while Earth decays, Cooper must decide to either stay, or risk never seeing his children again in order to save the human race by finding another habitable planet.",
            "source": "https://www.imdb.com/title/tt0816692/",
            "score": 0.7310055
          },
          {
            "imdbId": "tt0137523",
            "title": "Fight Club",
            "year": 1999,
            "plot": "A nameless first-person narrator attends support groups in an attempt to subdue his emotional state and relieve his insomniac state. When he meets Marla, another fake attendee of support groups, his life seems to become a little more bearable. However, when he associates himself with Tyler he is dragged into an underground fight club and soap-making scheme. Together the two men spiral out of control and engage in competitive rivalry for love and power.",
            "source": "https://www.imdb.com/title/tt0137523/",
            "score": 0.72578084
          }
        ]
      }
    ]
  }
}
```

## Agent’s actual answer

From our imported collection, **Shutter Island** shares an actor with *Inception* while exploring a very different set of themes. 

- **Shared Connection:** **Leonardo DiCaprio** stars in both films (playing the dream-extracting thief Dom Cobb in *Inception* and U.S. Marshal Teddy Daniels in *Shutter Island*). Note that the dataset's cast information is limited to listed OMDb cast members.
- **Theme & Plot Comparison:** While *Inception* focuses on science-fiction corporate espionage, dream sharing, and planting ideas within the subconscious, *Shutter Island* (2010) is a psychological mystery set in 1954. It follows Teddy Daniels as he investigates the disappearance of a patient from a remote asylum for the criminally insane, leading him to confront a web of sinister hospital treatments, a hurricane isolating the island, and doubts about his own sanity and memory. 

- IMDb reference for *Shutter Island*: [https://www.imdb.com/title/tt1130884/](https://www.imdb.com/title/tt1130884/)
