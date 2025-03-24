# Take-Home Assignment: Cognitive Assessment API

## Project Overview

You are tasked with building a RESTful API for a cognitive assessment platform. Users will submit short journal entries (text), and the API will analyze the text using a simple LIWC (Linguistic Inquiry and Word Count) algorithm to generate a score. This score will reflect the presence of specific words related to predefined categories, such as positive and negative emotions.

## Technology Stack

* **Preferred:** Python with Flask for the API, and a relational database (e.g., SQLite or PostgreSQL).
* **Alternative Stack:** If you prefer another stack (e.g., Node.js with Express, Java with Spring Boot), you may use it. Please justify your choice in the `README.md` and ensure it meets all project requirements (e.g., RESTful API, authentication, database integration).

## Functionality Requirements

* **User Management:**
    * Register and authenticate users using JWT tokens.

* **Journal Management:**
    * Submit journal entries (text).
    * Retrieve scores based on the analysis of the journal text.

## How to Calculate the Score

## Scoring Mechanism

The scoring mechanism is based on a simplified version of the LIWC algorithm. Here’s how it works:

**Concept**

The method calculates a score by counting how often specific words from a journal entry appear in predefined categories, such as "positive emotions" or "negative emotions." These categories are based on a dictionary that lists words associated with each one.

**Steps:**

1.  **Define a small set of categories and associated words.** For example:
    * **Positive Emotions:** "happy," "joy," "love"
    * **Negative Emotions:** "sad," "angry," "fear"
2.  **When a user submits a journal entry, tokenize the text** (split it into individual words).
3.  **Count how many words in the text match the words in each category.** Each word in the text is compared to the word lists in the dictionary. If a word matches one in a category, that category’s score increases by 1. This step is repeated for every word in the entry.
4.  **Assign a score based on these counts.** You can:
    * Calculate a score per category (e.g., number of positive emotion words).
    * Compute a total score by summing the counts across all categories.

**Example:**

Submitted Text: "I am happy today, but I was sad yesterday."

**Analysis:**

* **Positive Emotions:** 1 ("happy")
* **Negative Emotions:** 1 ("sad")

**Score Options:**

* Return category scores: `{ "positive": 1, "negative": 1 }`
* Return total score: 2 (sum of matches)

**Word Dictionary**

The dictionary is structured as a JSON object with four categories: "positive_emotion," "negative_emotion," "social," and "cognitive." Each category contains a list of at least 50 words to meet the requirement of 200 words total.

{
  "positive_emotion": [
    "happy", "joy", "love", "excited", "content", "pleased", "grateful", "hopeful", "proud", "amused",
    "cheerful", "delighted", "optimistic", "enthusiastic", "satisfied", "blissful", "ecstatic", "gleeful",
    "jubilant", "merry", "radiant", "thrilled", "upbeat", "vivacious", "zestful", "buoyant", "elated",
    "exhilarated", "lighthearted", "overjoyed", "rapturous", "triumphant", "euphoric", "exultant", "festive",
    "jolly", "jovial", "mirthful", "peppy", "perky", "playful", "sparkling", "sunny", "vibrant", "whimsical",
    "winsome", "zany", "carefree", "ebullient", "effervescent", "exuberant"
  ],
  "negative_emotion": [
    "sad", "angry", "fear", "anxious", "depressed", "frustrated", "worried", "upset", "disappointed", "guilty",
    "ashamed", "lonely", "miserable", "gloomy", "desperate", "hopeless", "bitter", "resentful", "irritated",
    "enraged", "furious", "aggravated", "annoyed", "disgruntled", "displeased", "exasperated", "incensed",
    "indignant", "outraged", "vexed", "apprehensive", "dreadful", "frightened", "panicked", "petrified",
    "terrified", "alarmed", "shocked", "horrified", "dismayed", "distressed", "grieved", "heartbroken",
    "melancholy", "mournful", "sorrowful", "woeful", "despondent", "disheartened", "forlorn", "pessimistic"
  ],
  "social": [
    "friend", "family", "team", "community", "partner", "colleague", "neighbor", "acquaintance", "ally", "companion",
    "confidant", "mate", "peer", "supporter", "advocate", "backer", "benefactor", "comrade", "crony", "pal",
    "associate", "collaborator", "co-worker", "classmate", "roommate", "playmate", "soulmate", "spouse", "sibling",
    "parent", "child", "relative", "kin", "clan", "tribe", "group", "club", "society", "organization", "network",
    "circle", "crew", "gang", "posse", "squad", "unit", "band", "troop", "assembly", "congregation", "gathering"
  ],
  "cognitive": [
    "think", "know", "believe", "understand", "realize", "consider", "contemplate", "ponder", "reflect", "analyze",
    "evaluate", "assess", "judge", "decide", "conclude", "deduce", "infer", "reason", "rationalize", "speculate",
    "hypothesize", "theorize", "postulate", "conjecture", "surmise", "guess", "estimate", "calculate", "compute",
    "measure", "quantify", "qualify", "compare", "contrast", "differentiate", "distinguish", "identify", "recognize",
    "recall", "remember", "recollect", "retrieve", "forget", "ignore", "overlook", "neglect", "misunderstand",
    "confuse", "bewilder", "perplex", "puzzle"
  ]
}
  
**Implementation Tips**

* **Storage:** Store the categories and their associated words either in a database (e.g., a table with columns for category and word) or in a configuration file (e.g., JSON or YAML).
* **Processing:** When a journal entry is submitted:
    1.  Convert the text to lowercase and split it into words (tokenization).
    2.  Compare each word against the predefined word lists for each category.
    3.  Tally the matches and compute the score.
* **Response:** Return the score(s) in the API response, either as a single total or broken down by category, depending on your design choice.

## Project Structure

Organize your project logically. A suggested structure for Python/Flask is:

```bash
/project-root
  /app
    /models       # Database models (e.g., User, Journal, Category, Word)
    /routes       # API endpoints
    /services     # Business logic (e.g., score calculation)
    /tests        # Unit tests
  Dockerfile      # Docker configuration
  requirements.txt # Python dependencies (if using Python)
  README.md       # Project documentation
```

## Endpoints

Implement these core RESTful API endpoints:

| Method | Endpoint             | Description                  | Authentication |
|--------|----------------------|------------------------------|----------------|
| POST   | /users               | Register a new user          | None           |
| POST   | /login               | Authenticate and return a JWT token | None           |
| POST   | /journals            | Submit a new journal entry   | JWT            |
| GET    | /journals/{id}/score | Retrieve the score for a journal entry | JWT            |

Use JSON for request and response bodies.

Include proper error handling (e.g., 400 for bad requests, 401 for unauthorized).

## Example API Flow

**User Submission:**

  - Endpoint: POST /journals
  - Request Body: { "text": "I am happy today, but I was sad yesterday." }
  - Authentication: JWT token require

**Processing:**

  - Tokenize: ["i", "am", "happy", "today", "but", "i", "was", "sad", "yesterday"]
  - Match:
    - "happy" → Positive Emotions (+1)
    - "sad" → Negative Emotions (+1)

**Response:**

    { "journal_id": 1, "score": { "positive": 1, "negative": 1 } }

    Or: 
    
    { "journal_id": 1, "score": 2 } (if using total)


## Deliverables

* **GitHub Repository Link:**
    * Source Code
    * Dockerfile
    * Unit Tests
    * `README.md` with:
        * Setup Instructions
        * Docker Commands (build/run)
        * Testing Instructions (unit tests, API examples)
        * Alternative Stack Justification (if applicable)
* **Code Quality:**
    * Clean, maintainable code with error handling.
* **Verification:**
    * Functional Docker container.
    * Working API, including accurate score calculation.


