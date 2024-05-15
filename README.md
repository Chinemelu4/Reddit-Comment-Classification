# Reddit-Comment-Classification

### My Approach
My approach for Machine learning projects is usually datacentric rather than model-centric. While there are very powerful deep learning models, I find that getting the training data quality and preparation optimal is usually more important. Hence my need to source a highly represenattove data before I began. So I did the following:

1. I webscraped several thousand of comments from different relevant reddit communities eg ('medicine', 'surgery', 'veterinary', 'veterinaryprofession', 'vet') using the PRAW library. Below is my function to do that:

```python
import pandas as pd
import numpy as np
import praw

def extract_comments_to_dataframe(subreddit):
    subreddit = reddit.subreddit(subreddit)
    hot = subreddit.hot(limit=None)
    
    data = {'Author Occupation': [], 'Comment': []}
    
    for submission in hot:
        submission.comments.replace_more(limit=None) 
        for comment in submission.comments.list():
            author_occupation = comment.author_flair_text if comment.author_flair_text else "Unknown"
            comment_text = comment.body
            data['Author Occupation'].append(author_occupation)
            data['Comment'].append(comment_text)
    
    df = pd.DataFrame(data)
    return df
```
2. I then combined the data
3. I ensured the consistency in naming of the 'author occupation' columns, i.e, MD, Doctor, Medical Doctor, US doctor, etc are mapped to a single name like "Doctor" same thing for veterinarian, veterinary doctor, vet, animal doctor, US Vet-Small Animal, UK vet etc are mapped to a single name "Veterinarian". 

4. I removed rows where the occupation of author is unknown
5. Other values mapped to "Others"
6. I balanced the dataset
7. Remove stopwords
8. Remove special characters and punctuations

By Doing this I was able to use a simpler algorithm to achieve good accuracy on classifying the comments to the occupation of the author. Other more complex algorithms like BERT achieved a slightly better accuracy but the tradeoff between the computational resources and time needed to train the model was not worth the marginal improvement in the model's accuracy.

You can find my training data here [Training Data](https://drive.google.com/drive/folders/14dXdcPJ8JPJurrbshoZTV4ukOhDwho_g?usp=sharing)
