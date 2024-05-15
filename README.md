# Reddit-Comment-Classification

### My Approach
My approach for Machine learning projects is usually datacentric rather than model-centric. While there are very powerful deep learning models, I find that getting the training data quality and preparation optimal is usually more important. Hence my need to source a highly represenattove data before I began. So I did the following:

1. I webscraped several thousand of comments from different relevant reddit communities eg ('medicine', 'surgery', 'veterinary', 'veterinaryprofession', 'vet') using the PRAW library. Below is my function to do that:

'''def extract_comments(subreddit):
    subreddit = reddit.subreddit('veterinaryprofession')
    hot = subreddit.hot(limit=None)

    
    with open(f'{subreddit}.csv', 'w', newline='', encoding='utf-8') as csvfile:
        csv_writer = csv.writer(csvfile)
        
        csv_writer.writerow(['Author Occupation', 'Comment'])
        
        for submission in hot:
            submission.comments.replace_more(limit=None) 
            for comment in submission.comments.list():
                author_occupation = comment.author_flair_text if comment.author_flair_text else "Unknown"
                comment_text = comment.body
                csv_writer.writerow([author_occupation, comment_text])
    returnf'{subreddit}.csv'
'''

2. I ensured the consistency in naming of the 'author occupation' columns, i.e, MD, Doctor, Medical Doctor, US doctor, etc
are mapped to a single name like "Doctor" same thing for veterinarian, veterinary doctor, vet, animal doctor,
US Vet-Small Animal, UK vet etc are mapped to a single name "Veterinarian". 

3. I removed rows where the occupation of author is unknown
4. Other values mapped to "Others"
5. I balanced the dataset
6. Remove stopwords
7. Remove special characters and punctuations

By Doing this I was able to use a simpler algorithm to achieve good accuracy on classifying the comments to the occupation of the author. Other more complex algorithms like BERT achieved a slightly better accuracy but the tradeoff between the computational resources and time needed to train the model was not worth the marginal improvement in the model's accuracy.

You can find my training data here
