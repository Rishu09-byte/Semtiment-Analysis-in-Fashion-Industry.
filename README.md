# Semtiment-Analysis-in-Fashion-Industry.
This project utilizes AI to analyze consumer behavior in the fashion industry. It focuses on sentiment analysis of widely used keywords and predictive analytics to enhance personalization and marketing strategies. Key tools include Python, and Power BI for data processing and visualization.

import matplotlib.pyplot as plt
from collections import Counter

data = [
    {"ReviewID": 1, "Review": "I love the quality of this dress, but it's too expensive."},
    {"ReviewID": 2, "Review": "The shoes are comfortable and stylish. Highly recommend!"},
    {"ReviewID": 3, "Review": "This jacket is poorly made and fell apart after a week."},
    {"ReviewID": 4, "Review": "Great value for the price. Will buy again."},
    {"ReviewID": 5, "Review": "The material of the shirt is not what I expected. Disappointed."}
]

positive_words = ['love', 'great', 'comfortable', 'stylish', 'recommend']
negative_words = ['expensive', 'poorly', 'fell apart', 'disappointed']

def analyze_sentiment(review):
    # Initialize scores
    positive_score = 0
    negative_score = 0
    words = review.lower().split()
    for word in words:
        if word in positive_words:
            positive_score += 1
        elif word in negative_words:
            negative_score += 1
    return positive_score - negative_score

sentiments = []

all_reviews = ""
for entry in data:
    sentiment_score = analyze_sentiment(entry['Review'])
    sentiments.append(sentiment_score)
    entry['SentimentScore'] = sentiment_score
    entry['Sentiment'] = 'Positive' if sentiment_score > 0 else ('Negative' if sentiment_score < 0 else 'Neutral')
    all_reviews += entry['Review'] + " "

for entry in data:
    print(f"Review ID: {entry['ReviewID']}, Review: {entry['Review']}, Sentiment: {entry['Sentiment']}")

sentiment_counts = Counter([entry['Sentiment'] for entry in data])
labels = list(sentiment_counts.keys())
sizes = list(sentiment_counts.values())

plt.figure(figsize=(18, 6))

plt.subplot(1, 3, 1)
plt.bar(labels, sizes, color=['green', 'red', 'blue'])
plt.title('Sentiment Distribution of Fashion Reviews')
plt.xlabel('Sentiment')
plt.ylabel('Count')


plt.subplot(1, 3, 2)
plt.pie(sizes, labels=labels, colors=['green', 'red', 'blue'], autopct='%1.1f%%', startangle=140)
plt.axis('equal')
plt.title('Sentiment Distribution (Pie Chart)')

plt.subplot(1, 3, 3)
plt.plot(range(1, len(sentiments) + 1), sentiments, marker='o', linestyle='-')
plt.title('Sentiment Score Trend')
plt.xlabel('Review ID')
plt.ylabel('Sentiment Score')
plt.grid(True)

plt.tight_layout()
plt.show()
