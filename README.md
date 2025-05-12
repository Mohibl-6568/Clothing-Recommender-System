# Clothify Interactive Recommender System Documentation
## Overview
The Clothify Interactive Recommender System is a terminal-based personalized fashion recommendation engine. It uses TF-IDF-based tag vectorization, user interaction feedback, time decay, and gender-based filtering to suggest relevant clothing items from a dataset.
## Features
| Feature                         | Description                                                        |
| ------------------------------- | ------------------------------------------------------------------ |
| **User login**                  | Users log in by entering a username and selecting a gender.        |
| **Gender filtering**            | Shows only items relevant to the user's gender (Male/Female/Both). |
| **TF-IDF tag vectorization**    | Transforms item tags into weighted vectors using TF-IDF.           |
| **Feedback loop**               | Users provide feedback (like/dislike) for each item.               |
| **Tag-based personalization**   | Updates user profile based on liked/disliked tags.                 |
| **Time decay**                  | More recent interactions have more influence on recommendations.   |
| **Multi-round recommendations** | Three recommendation rounds with top-N suggestions in each.        |

## Code Structure
### 1. Data Preprocessing
Loads CSV data.
Drops invalid titles and owner column.
Standardizes the For (Male, Female, both) field.
Merges tag columns into a single combined_tags field.

### 2. User Login & Gender Filter
Prompts the user for username and gender.
Filters the DataFrame accordingly.

### 3. TF-IDF Vectorization
Converts combined tags into TF-IDF vectors.

### 4. User Profile with Time Decay
Time Decay Formula:
decayed_weight = original_weight × 𝑒−𝜆×age
decayed_weight=original_weight×e 
−λ×age
 
λ (lambda) controls how quickly old interactions decay. (e.g., 0.01)
for weight, timestamp in entries:
    age = now - timestamp
    decayed_weight = weight * math.exp(-DECAY_LAMBDA * age)
The more recent the interaction, the higher its influence.

### 5. Recommendation Engine
Candidate Selection:
Compute cosine similarity between user vector and TF-IDF item vectors.
Sort unseen items by similarity score.

similarities = cosine_similarity(user_vector, tfidf_matrix).flatten()

Feedback Collection:
User enters 'L' (Like) or 'D' (Dislike).
Updates user_tag_history with new feedback and current timestamp.

### 6. Recommendation Loop
Repeats the process for 3 rounds.

Displays top 5 recommended items in each round.

Prints updated tag history and count of seen items.

| Parameter      | Description                             | Default |
| -------------- | --------------------------------------- | ------- |
| `DECAY_LAMBDA` | Time decay rate (higher = faster decay) | `0.01`  |
| `rounds`       | Number of recommendation rounds         | `3`     |
| `top_n`        | Number of items per round               | `5`     |

### Limitations
Runs in terminal only (no GUI).
No persistent user profiles across sessions.
Time units in seconds (session-based decay, not calendar-based).



