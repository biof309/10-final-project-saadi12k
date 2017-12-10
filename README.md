# 10-final-project-saadi12k
10-final-project-saadi12k created by GitHub Classroom
{
 "cells": [
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# Our objective is to analyze the Data gathered using the streaming API"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "#### We will amke use of 4 different libraries: 1) json for parsing the data 2) pandas for data manipulation 3) matplotlib for charting the data and 4) re for regular expressions"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "we need to first set our directory to the specific folder (this is to be changed to fit each situation)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 1,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "C:\\Users\\Hassan Saadi\\Desktop\\Python_Final\n"
     ]
    }
   ],
   "source": [
    "cd Desktop/Python_Final/"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 2,
   "metadata": {},
   "outputs": [],
   "source": [
    "# we first import the libraries\n",
    "import json\n",
    "import pandas as pd\n",
    "import matplotlib.pyplot as plt\n",
    "import re\n",
    "import numpy as np"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "#### we define a function that will first check the data set for text in individual tweets and then within the text it will seasearch for the word specified\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 3,
   "metadata": {},
   "outputs": [],
   "source": [
    "# we define a function that will first check the data set for text in individual tweets and then within the text it will seasearch for the word specified\n",
    "def word_in_text(word, text):\n",
    "    if text == None:\n",
    "        return False\n",
    "    word = word.lower()\n",
    "    text = text.lower()\n",
    "    match = re.search(word, text)\n",
    "    if match:\n",
    "        return True\n",
    "    return False"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## Reading Tweets\n",
    "\n",
    "#### we first import the json file that we collected using the streaming API"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 4,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "Reading Tweets\n",
      "\n",
      "The number of tweets collected is 167704\n"
     ]
    }
   ],
   "source": [
    "# Reading Tweets\n",
    "# we first import the json file that we collected using the streaming API\n",
    "print('Reading Tweets\\n')\n",
    "tweets_data_path = \"twitter_data.txt\"\n",
    "\n",
    "tweets_data = []\n",
    "tweets_file = open(tweets_data_path, \"r\")\n",
    "for line in tweets_file:\n",
    "    try:\n",
    "        tweet = json.loads(line)\n",
    "        tweets_data.append(tweet)\n",
    "    except:\n",
    "        continue\n",
    "\n",
    "print (\"The number of tweets collected is \" + str(len(tweets_data)))\n"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## Structuring Tweets\n",
    "#### We utilize pandas DataFrames for this purpose"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 5,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "Structuring Tweets\n",
      "\n"
     ]
    }
   ],
   "source": [
    "# Structuring Tweets\n",
    "#here we make use of pandas to define columns with data from the json file\n",
    "#we look at the text, language (lang), and country of origin regarding the tweet\n",
    "print('Structuring Tweets\\n')\n",
    "tweets = pd.DataFrame()\n",
    "\n",
    "#tweets['YOUR STRING HERE'] = list(map(lambda tweet: tweet.get('YOUR STRING HERE', None),tweets_data))\n",
    "tweets['text'] = list(map(lambda tweet: tweet.get('text', None),tweets_data))\n",
    "tweets['lang'] = list(map(lambda tweet: tweet.get('lang', None),tweets_data))\n",
    "tweets['country'] = list(map(lambda tweet: tweet.get('place',{}).get('country', None) if tweet.get('place') != None else None,tweets_data))"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## Analyzing Tweets by Language\n",
    "#### Here we get the language of each tweet and plot the top 10 using matplotlib"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 6,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "Analyzing tweets by language\n",
      "\n",
      "The number of languages used: 46\n"
     ]
    },
    {
     "data": {
      "image/png": "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\n0KDPmknPDwAAAABJRU5ErkJggg==\n",
      "text/plain": [
       "<matplotlib.figure.Figure at 0x14dfc9f1860>"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    }
   ],
   "source": [
    "# Analyzing Tweets by Language\n",
    "#here we get the language of each tweet and plot the top 10\n",
    "print('Analyzing tweets by language\\n')\n",
    "tweets_by_lang = tweets['lang'].value_counts()\n",
    "\n",
    "fig, ax = plt.subplots()\n",
    "ax.tick_params(axis='x', labelsize=15)\n",
    "ax.tick_params(axis='y', labelsize=10)\n",
    "ax.set_xlabel('Languages', fontsize=15)\n",
    "ax.set_ylabel('Number of tweets' , fontsize=15)\n",
    "ax.set_title('Top 5 languages', fontsize=15, fontweight='bold')\n",
    "tweets_by_lang[:10].plot(ax=ax, kind='bar', color='red')\n",
    "plt.savefig('tweet_by_lang', format='png')\n",
    "print( \"The number of languages used: \" + str(len(tweets_by_lang)))\n",
    "plt.show()"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## Analyzing Tweets by Country\n",
    "#### Here we do a similar comparison as above but look at the country where the tweet originated (if any) and plot it again using matplotlib"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 7,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "Analyzing tweets by country\n",
      "\n"
     ]
    },
    {
     "data": {
      "image/png": "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\n",
      "text/plain": [
       "<matplotlib.figure.Figure at 0x14e755ef080>"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    }
   ],
   "source": [
    "# Analyzing Tweets by Country\n",
    "#here we do a similar comparison as above but look at the country where the tweet originated (if any)\n",
    "print('Analyzing tweets by country\\n')\n",
    "tweets_by_country = tweets['country'].value_counts()\n",
    "\n",
    "fig, ax = plt.subplots()\n",
    "ax.tick_params(axis='x', labelsize=15)\n",
    "ax.tick_params(axis='y', labelsize=10)\n",
    "ax.set_xlabel('Countries', fontsize=15)\n",
    "ax.set_ylabel('Number of tweets' , fontsize=15)\n",
    "ax.set_title('Top 5 countries', fontsize=15, fontweight='bold')\n",
    "tweets_by_country[:20].plot(ax=ax, kind='bar', color='green')\n",
    "plt.savefig('tweet_by_country', format='png')\n",
    "plt.show()"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## Analysis of mentions per tweet for each individual sports\n",
    "\n",
    "#### now we introduce columns for the specific sport to analyze the mention of each sport and asses how many times the keyword is mentioned"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 8,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "Analyzing tweets by sports\n",
      "\n"
     ]
    }
   ],
   "source": [
    "#now we introduce columns for the specific sport to analyze the mention of each sport\n",
    "print('Analyzing tweets by sports\\n')\n",
    "tweets['basketball'] = tweets['text'].apply(lambda tweet: word_in_text('basketball', tweet))\n",
    "tweets['soccer'] = tweets['text'].apply(lambda tweet: word_in_text('soccer', tweet))\n",
    "tweets['football'] = tweets['text'].apply(lambda tweet: word_in_text('football', tweet))"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "#### The raw mentions of each sport"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 9,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "The total number of mention of each respective sport are as follows:\n",
      "basketball 29044\n",
      "soccer 17700\n",
      "football 67411\n"
     ]
    }
   ],
   "source": [
    "#Here we have the frequency of each keyword used, in this case the name of the three sports\n",
    "print(\"The total number of mention of each respective sport are as follows:\")\n",
    "print('basketball', tweets['basketball'].value_counts()[True])\n",
    "print(\"soccer\", tweets['soccer'].value_counts()[True])\n",
    "print(\"football\", tweets['football'].value_counts()[True])"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### The ranking of each respective sport by their names"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 10,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "image/png": "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\nYII=\n",
      "text/plain": [
       "<matplotlib.figure.Figure at 0x14e755ef7b8>"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    }
   ],
   "source": [
    "#similar to before, we plot the ranking of each sport respectively\n",
    "type_sport = ['basketball', 'soccer', 'football']\n",
    "tweets_by_type_sport = [tweets['basketball'].value_counts()[True], tweets['soccer'].value_counts()[True], tweets['football'].value_counts()[True]]\n",
    "\n",
    "x_pos = list(range(len(type_sport)))\n",
    "width = 0.5\n",
    "fig, ax = plt.subplots()\n",
    "plt.bar(x_pos, tweets_by_type_sport, width, alpha=1, color='g')\n",
    "ax.set_ylabel('Number of tweets', fontsize=15)\n",
    "ax.set_title('Ranking: basketball vs. soccer vs. football (english + american) (Raw data)', fontsize=10, fontweight='bold')\n",
    "ax.set_xticks([p + 0.4 * width for p in x_pos])\n",
    "ax.set_xticklabels(type_sport)\n",
    "plt.grid()\n",
    "plt.savefig('tweets_by_type_sport_1', format='png')\n",
    "plt.show()"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## Analysis of mentions per tweet for a star individual in their respective sport\n",
    "\n",
    "#### now we introduce columns for each individual in question and access if the keyword (name) is mentioned in the respective tweet"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 11,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "Analyzing tweets by a popular player in each respective sport\n"
     ]
    }
   ],
   "source": [
    "#No we do a similar comparison but now with a popular name in each respective sport\n",
    "#first we define columns such that we can check if the givem name is mentioned in a tweet\n",
    "print('Analyzing tweets by a popular player in each respective sport')\n",
    "tweets['lebron'] = tweets['text'].apply(lambda tweet: word_in_text('lebron', tweet))\n",
    "tweets['ronaldo'] = tweets['text'].apply(lambda tweet: word_in_text('ronaldo', tweet))\n",
    "tweets['brady'] = tweets['text'].apply(lambda tweet: word_in_text('brady', tweet))"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### The mention of each individual in the data set"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 12,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "The total number of mention of each respective player are as follows:\n",
      "Lebron 199\n",
      "Ronaldo 1934\n",
      "Brady 51\n"
     ]
    }
   ],
   "source": [
    "#We first look at the mention for each player\n",
    "print(\"The total number of mention of each respective player are as follows:\")\n",
    "print('Lebron', tweets['lebron'].value_counts()[True])\n",
    "print(\"Ronaldo\", tweets['ronaldo'].value_counts()[True])\n",
    "print(\"Brady\", tweets['brady'].value_counts()[True])"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### The plot for ranking by star player"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 13,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "image/png": "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\nAAAAAElFTkSuQmCC\n",
      "text/plain": [
       "<matplotlib.figure.Figure at 0x14e76d79eb8>"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    }
   ],
   "source": [
    "#now we plot the data as before\n",
    "pop_players = ['lebron', 'ronaldo', 'brady']\n",
    "tweets_by_pop_players = [tweets['lebron'].value_counts()[True], tweets['ronaldo'].value_counts()[True], tweets['brady'].value_counts()[True]]\n",
    "\n",
    "x_pos = list(range(len(pop_players)))\n",
    "width = 0.5\n",
    "fig, ax = plt.subplots()\n",
    "plt.bar(x_pos, tweets_by_pop_players, width, alpha=1, color='g')\n",
    "ax.set_ylabel('Number of tweets', fontsize=15)\n",
    "ax.set_title('Ranking: Lebron vs. Ronaldo vs. Brady (Raw data)', fontsize=10, fontweight='bold')\n",
    "ax.set_xticks([p + 0.4 * width for p in x_pos])\n",
    "ax.set_xticklabels(pop_players)\n",
    "plt.grid()\n",
    "plt.savefig('tweets_by_pop_players', format='png')\n",
    "plt.show()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": []
  }
 ],
 "metadata": {
  "kernelspec": {
   "display_name": "Python 3",
   "language": "python",
   "name": "python3"
  },
  "language_info": {
   "codemirror_mode": {
    "name": "ipython",
    "version": 3
   },
   "file_extension": ".py",
   "mimetype": "text/x-python",
   "name": "python",
   "nbconvert_exporter": "python",
   "pygments_lexer": "ipython3",
   "version": "3.6.3"
  }
 },
 "nbformat": 4,
 "nbformat_minor": 2
}