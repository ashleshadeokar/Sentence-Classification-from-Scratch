# Sentence-Classification-from-Scratch

The full process will require the following steps:
1. Understand the task specification
2. Collect raw data
3. Annotate test and training data for development
4. Train and test models using this data
5. ”Deploy” your System

# The Task:
We will identify the framing of a given parapgraph/sentence in several languages. Effectively, this is a straight-forward text classification problem in a multilingual setting.

Input: The input to the model will be a text file with one paragraph/sentence per line. The text will already be tokenized using the spacy tokenizer,and you should not change the tokenization. 
An example of the input looks like this (notice the spacing of the punctuation):
Some economists say that immigrants , legal and illegal , produce a net economic gain , while others say that they create a net loss .

Output: The output of your model should be a .tsv file, with one sentence per line, a tab, and then the corresponding label.

# Data Collection
First Round of Data Labeling: You will collect/create a labeled dataset for our task of at least 150 sentences. Notice that this is a minimum number of data you need to annotate, but annotating more is allowed.

Package your annotations in a file named first data.tsv. The .tsv file should include the following five columns:

• text: the sentence that will be the input to the model

• language: the language of the sentence. If codemixed, use a comma-separated list of languages. Use either language names or language ISO-3 codes.

• label: the framing label of the sentence. Use either the label name (e.g. ”economic”) or the label ID according to the annotation schema.

      labels = ['Economic', 'Capacity and resources', 'Fairness and equality', 'Legality, constitutionality and jurisprudence', 'Policy prescription and evaluation', 'Crime and punishment', 'Security and defense', 'Health and safety', 'Quality of life', 'Cultural identity', 'Public opinion', 'External regulation and reputation', 'Political', 'Morality', 'Other']

• source: the source of the sentence. If scraped, provide the URL. If written by you or someone else, provide the author (or system) name.

To produce your final dataset, you will need to aggregate the annotations.  That means that for cases where they disagree, you will need to choose a single label. Use any method you like other than random label selection to perform this aggregation. You will need to create a file aggregated data.tsv,
with the same format as your first data.tsv.

To produce the final dataset, it was necessary to aggregate the annotations. Random selection of a label was not an option, as this could lead to unreliable results. I chose the label that I believed was most appropriate for the sentence. I created a new file named ”aggregated data.tsv.” This file has
the same format as the original “first data.tsv” and contains the final, agreed-upon labels for each sentence. This allowed me for a consistent dataset that is supposed to be used for analysis in the further part of the assignment.

# Model Training and Testing

The next step is to train a text classification model and try to get an estimation of how well it would perform, based on your data.
Follow the steps in the notebook to connect to a GPU, install the Huggingface transformers library, read-in the data, create train/dev/test splits, set hyperparameters, and then fine-tune your model.

After you’re satisfied with your hyperparameters, it’s time to evaluate your model on the test set!

I chose the ”nlptown/bert-base-multilingual-uncased-sentiment” model instead of the ”base-bert-uncased” model in order to improve performance since I was only getting 18 correct predictions with the given model. I used the first data.tsv, annotated data, seed.tsv as well as the augmented data combined to train the model. My experiments included altering the epoch number, learning
rate, dropout rate, and fine tuning the layers. In order to tune hyperparameters, I used a trial-and-error method of trying different values and then evaluating the model’s performance according to the results. I tried different batch sizes, such as 16, 32, and 64. I used the dropout function
to prevent overfitting during training. After experimenting with different values for the number of epochs, I observed that there was no significant change after a certain number of epochs. So I set the numner of epochs to 10. Different learning rates were tried like 3e-4, 4e-4 and I found
that 5e-5 gave the best result for my model. Furthermore, I adjusted the batch size and fine-tuned the layers in addition to the hyperparameters. The best results were obtained when I set the Finetine layers to 10. After changing the model and adjusting all the hyperparameters,the number of
correct predictaions was 102.The accuracy of the test set is 56% and that of the validation set is 64%. There was an increase in accuracy and number of correct predictions for the validation set.
you obtain.

# Error Analysis

Next, perform an error analysis on your model.
An error analysis is performed on a model in order to gain insight into what types of errors it makes and where improvements can be made. We can improve the model’s performance by identifying patterns or common linguistic properties the model struggles with by analyzing
the examples that the model gets wrong. The code for error analysis was written using the get validation function as reference and printed out 5 incorrect predictions. A lack of diversity in data may be a contributing factor to errors as there are few sentences in different languages
in my dataset. Paraphrases are used in most sentences because of data augmentation. There are also some sentences translated from English to another language and vice-versa. In addition, it is possible that the sentences are labeled incorrectly, resulting in wrong predictions being made because of the incorrect data used to train the model. Checking the labels of the training data can
improve the model’s performance. Training the model on more data by increasing, the size of the dataset. Furthermore, we can train the model on data that includes more sentences in different languages. It is also possible to improve results by changing the hyperparameter.

# Data Augmentation

To augment the data, I used a combination of paraphrasing and translation techniques. For paraphrasing, I took the sentences from the first data.tsv and rephrased them while keeping the original label ID and label names intact. This helped to increase the diversity of the training data
and reduced the chances of overfitting. I also used the ’google/pegasus-xsum’ model to augment my data. I generated over 800 sentences using this model and added it to my dataset. Additionally, since my dataset contained sentences in multiple languages, I used translation to convert
non-English sentences to English. This helped to increase the size of the dataset and improve the model’s performance on non-English text. After performing data augmentation, I trained the model on the augmented dataset and evaluated its performance on the test set. The results showed
that data augmentation resulted in improved performance on the test set compared to the model trained on the final data that consisted of first data and annotator data alone. I believe this happened because data augmentation helped to increase the diversity and size of the training dataset,
which in turn improved the model’s ability to generalize to new examples. By providing more varied examples, the model was able to learn more robust features of the training data.

# Model Deployment

The final ”deployment” of your model will consist of running your model over a random test set.

