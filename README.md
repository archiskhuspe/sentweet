# Sentweet — Twitter Sentiment Analysis

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/ArchisKhuspe/Twitter-Sentiment-Analysis-Using-Python/blob/main/twitter_sentiment_analysis.ipynb)
![Python](https://img.shields.io/badge/Python-3.8%2B-blue)
![License: MIT](https://img.shields.io/badge/License-MIT-green)

Sentweet is a Python notebook that fetches a Twitter/X user's most recent tweets, classifies each
tweet's sentiment as **positive**, **negative**, or **neutral**, and visualises the results
with a word cloud, a polarity/subjectivity scatter plot, and bar/pie charts.

> **Note:** This project uses [TextBlob](https://textblob.readthedocs.io/), a lexicon-based
> sentiment analyser. It applies a pre-built sentiment lexicon rather than a model that you
> train yourself, so no machine-learning training step is involved.

## Table of Contents

- [Features](#features)
- [Tech Stack](#tech-stack)
- [How It Works](#how-it-works)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Configuration](#configuration)
- [Usage](#usage)
- [Project Structure](#project-structure)
- [Limitations](#limitations)
- [License](#license)

## Features

- Fetches the most recent tweets from any public Twitter/X handle via the Twitter API.
- Cleans tweet text (removes `@mentions`, `#` symbols, `RT` markers, and hyperlinks).
- Computes **polarity** (negative ↔ positive) and **subjectivity** (factual ↔ opinionated)
  for every tweet using TextBlob.
- Classifies each tweet as Positive, Negative, or Neutral.
- Reports the percentage of positive and negative tweets.
- Visualises results with a word cloud, a polarity–subjectivity scatter plot, a bar chart,
  and a pie chart.

## Tech Stack

| Purpose                 | Library                |
| ----------------------- | ---------------------- |
| Twitter API access      | Tweepy                 |
| Sentiment analysis      | TextBlob               |
| Word cloud generation   | wordcloud              |
| Data handling           | pandas, NumPy          |
| Text cleaning           | re (standard library)  |
| Visualisation           | Matplotlib             |
| Credential loading      | python-dotenv          |

## How It Works

1. **Authenticate** — A Tweepy API client is created using Twitter API credentials loaded
   from environment variables.
2. **Fetch** — The most recent tweets for a given `screen_name` are retrieved with
   `api.user_timeline(...)`.
3. **Clean** — Each tweet is stripped of mentions, hashtag symbols, retweet markers, and URLs.
4. **Score** — TextBlob assigns a polarity and subjectivity score to each cleaned tweet.
5. **Classify** — Tweets are labelled Positive (`polarity > 0`), Neutral (`polarity == 0`),
   or Negative (`polarity < 0`).
6. **Visualise** — Results are summarised as a word cloud, scatter plot, bar chart, and pie chart.

## Prerequisites

- Python 3.8 or newer.
- A [Twitter/X Developer account](https://developer.twitter.com/) with API credentials
  (consumer key/secret and access token/secret).

## Installation

```bash
# Clone the repository
git clone https://github.com/ArchisKhuspe/Twitter-Sentiment-Analysis-Using-Python.git
cd Twitter-Sentiment-Analysis-Using-Python

# (Recommended) create and activate a virtual environment
python -m venv .venv
source .venv/bin/activate        # On Windows: .venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt

# TextBlob needs its corpora the first time you use it
python -m textblob.download_corpora
```

## Configuration

Twitter credentials are read from environment variables and are **never** committed to the
repository. Copy the example file and fill in your own keys:

```bash
cp .env.example .env
```

Then edit `.env`:

```dotenv
TWITTER_CONSUMER_KEY=your_consumer_key_here
TWITTER_CONSUMER_SECRET=your_consumer_secret_here
TWITTER_ACCESS_TOKEN=your_access_token_here
TWITTER_ACCESS_TOKEN_SECRET=your_access_token_secret_here
```

The `.env` file is listed in [.gitignore](.gitignore) so your secrets stay local.
On Google Colab, store these as Colab secrets or set them with `os.environ` instead.

## Usage

Open the notebook and run the cells top to bottom:

```bash
jupyter notebook twitter_sentiment_analysis.ipynb
```

Or open it directly in Google Colab using the badge at the top of this README.

To analyse a different account or change the number of tweets, edit the `screen_name` and
`count` arguments in the tweet-fetching cell:

```python
posts = api.user_timeline(screen_name="@JoeBiden", count=50, lang="en", tweet_mode="extended")
```

## Project Structure

```
.
├── twitter_sentiment_analysis.ipynb   # Main analysis notebook
├── report.docx                        # Detailed project report
├── requirements.txt                   # Python dependencies
├── .env.example                       # Template for API credentials
├── .gitignore                         # Ignores secrets, caches, OS files
├── LICENSE                            # MIT license
└── README.md                          # This file
```

## Limitations

- **Lexicon-based**: TextBlob uses a fixed sentiment lexicon, so it can miss sarcasm, slang,
  emoji, and context-dependent meaning.
- **English only**: Tweets are fetched and scored in English.
- **Twitter API access**: The notebook uses the v1.1 `user_timeline` endpoint, which requires
  appropriate API access; availability and rate limits depend on your developer tier.

## License

This project is released under the [MIT License](LICENSE).
