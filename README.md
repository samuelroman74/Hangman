## This README reads more like a paper in order to give full context as to the project and what was sought to be accomplished, as the purpose and method of discovery are equally important to the code in this case.

This study/project primarily utilized K Neighbor Regressors and a dataset of 20k words that were adapted and modified for appropriate use in the context of creating an ML model that could play hangman. Unsurprisingly, there was some difficulty in allowing the model to predict values when the premise of the data is that the majority is missing, provided many challenges. This project seeks to be more of a proof of concept, however, in which it succeeds. It mainly demonstrates that there is potential in this project, especially when one adopts more adept methods (e.g. different approaches and, in some cases, stronger computing power).

# Introduction:
Large Language Models (LLMs) are in the public eye now more than ever before, especially with the onset of such things like GPT-4 and LLaMA. These models require billions of parameters, if not trillions of them, in order to be effective/meet the standards expected by their development companies, and they are trained on massive stores of text and literature. They consume data hungrily, and rightfully so, as their sophistication requires this.

However, though Large Language Models have dominated at least the general public’s focus regarding machine learning, there is something to be said for breaking that concept apart even further. That is to say, if the very concept of LLMs require piecing together sentences from words given as much data as possible, I wanted to see if it was possible to do the opposite: piecing together words from letters with as little information possible. Still giving it data of course, as that is inevitable, but training the model itself to extrapolate from as little information as possible. 

When it came to this, a classic game of hangman came to mind, as it very much encapsulates that concept. From as little information as possible, guess a letter, and then use that new information to formulate your guesses and so on. Though it would require some flexibility and creativity in formulating the data and determining the accuracy of the model (a core feature of hangman is that there is allowance for mistakes, and they are indeed expected), to be able to train a model to do so would be a proof of concept as well as a challenge for me personally, as a student. 

In the end, and especially when formatted in specific terms, the project really seeks to answer the question, “How well can Machine Learning play a game of hangman and extrapolate from wildly incomplete data?”

# Methodology:
Feed data into separate program to properly format it for later model training.
Split data into training and testing sets.
Train models and determine accuracy.
Make tweaks regarding parameters of models where necessary.
Make a for loop for individual input, so that the model can guess a letter and be trained on “new” set with included letter if guess is correct.
I will now go into more depth regarding methodology and the specific mechanics implemented (as well as the reasons for doing so). 

As I mentioned before, a lot of the difficulty in this project came from formatting the data and determining the accuracy of models rather than training the model itself. A 20k dataset (which one can find in references) was used for this task, simply because many of the other datasets were simply too large to be consistently manipulating on my computer (a very large limitation of the project). This dataset contained a wide variety of words from the English language.

However, not every word in this dataset could be used, especially to play a game of hangman. One of my greatest concerns was the length of words. How effective could a game of hangman be played with the word “the”, for example? Because I wanted the model/program to gain more information about the word as it went, allowing three letter words and under felt like it would confound the program, maybe even leaning into some underfitting when all was said and done.

A second large concern, even more prominent than the first, was that the words were just that: words. I would need to find a way to shift the words in the dataset to a format the model could be trained on efficiently. 

Furthermore, the core mechanic of hangman is that the letters are missing. Whichever system I used to encode the words, I’d have to find a way to meaningfully separate it into the letters the model knows are in the word (x) and which letters are left in the word to be guessed (y).

To solve these problems, I created a program that took each word in from the original dataset, discarded if it was too short, encoded it into the format I had decided on, and split it up into what would be x and y randomly. The format in question was a simple 1×26 array for each word. If a letter was present in the word, it was denoted as a 1 in the array at the proper position for that letter (index 0 was “a”, index 1 was “b”, and so on). One-hot encoding, which was my original first thought for the problem, was undoable due to the processing power on my computer. Limitations regarding this method will be discussed later, but it functioned as a serviceable solution.

I created two datasets, one that split word-knowledge (i.e. the amount of letters in x vs y) equally, and one that heavily favored y, as a real-life game of hangman would. The latter dataset specifically split it up (randomly) so that x would have ¼ of the letters. I then created two models, both of them K-Neighbor Regressors. They each had an n_neighbors of 100, a value received after manual tweaking and adjustment, as well as uniformly distributed weights (because of the formatting, distance would not be nearly as viable). One model was tested on the first dataset, the second model on the latter, and they were tested for accuracy similarly.

With regards to accuracy, different testing mechanisms were implemented. Specifically, I tested whether the highest value the regressor predicted was in the word, how many of the four highest values were in the word (four chosen because that’s the minimum length of a word), and whether one of the six highest values (six chosen because you frequently get 6 guesses in hangman) were somewhere in the word.

# Results and Discussion:
As expected, the first two tests (whether the highest value predicted was in the word and how many of the six highest values were in the word), had relatively low accuracies for both models. The first model, the one trained on a more even split of letters between x and y, would have its top guess be in the word about 41.3% of the time, and when asked for its top 4, 33.5% of its guesses were in the word. Whether one of the six highest values were in the word garnered a much higher score of 89.4%. 

Results were somewhat similar for the second model. Its top guess was accurate 27.7% of the time, top 4 test resulted in a 19.7% accuracy, and whether one of six calculated values were in a word yielded an accuracy of 67.6%. 

Out of curiosity, I did test the models on each other’s datasets to see how they would perform, and results were very comparable and almost exactly alike. I also implemented a Multilayer Perceptron out of curiosity, and results were again almost identical. Since this is not part of the official project, however, I will leave deep discussion of said results for another time.

This relatively low accuracy, especially in the more selective tests, is not entirely unexpected due to the limitations of the project and my computer’s processing power. Without the ability to one-hot encode words, a lot of potential data the model could train on/learn from such as placement of letters in the word and/or repetition of said letters is unable to be gathered. Similarly, because of the nature of hangman and how I had to split up the data, the accuracy scores are not, themselves, completely accurate. A four-letter word might have repeating letters, and thus the accuracy scoring technique may result in an invalid penalty, simply because the model has no more valid letters to predict. In further iterations of this project, this would have to be corrected.

# Conclusion:
In conclusion, this works very well as a proof of concept. Especially when there is room for error, even this rudimentary implementation of models for a hangman game will guess a letter from a word with better efficiency than randomization. Further work on stronger computers will result in allowing the model to learn from placement of letters and account for repeated letters. Even relatively crude implementations such as this can extrapolate from data that is meant to be incomplete, and it can play very good games of hangman.

# References:
https://github.com/methi1999/hangman/blob/master/dataset/250k.txt

https://github.com/Azure/Hangman/blob/master/Train%20a%20Neural%20Network%20to%20Play%20Hangman.ipynb

https://stackoverflow.com/questions/9942861/optimal-algorithm-for-winning-hangman

https://en-academic.com/dic.nsf/enwiki/8683
