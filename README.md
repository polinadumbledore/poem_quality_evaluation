# Poem Quality Evaluation

## Main Idea
The system evaluates poems based on several parameters: rhythm, novelty, repetition, meaningfulness, emotionality, and grammar. The project comprises several scripts: the first one counts repetition and novelty (counting novelty is only possible if a poem is part of a corpus; the script evaluates novelty by comparing the poem to others in the corpus), the second one evaluates rhythm, and the last one is a model trained on BERT to classify the poem based on three labels: meaningfulness, emotionality, and grammar.

## File navigation
* [poems_generation](https://github.com/polinadumbledore/poem_quality_evaluation/blob/main/poems_generation):
    * [generated_poems.json](https://github.com/polinadumbledore/poem_quality_evaluation/blob/main/poems_generation/generated_poems.json) – corpus of generated poems
    * [generation_sources.txt](https://github.com/polinadumbledore/poem_quality_evaluation/blob/main/poems_generation/generation_sources.txt) – list of all sources used for generation (with prompts)
    * [poems_from_pictures_generation.ipynb](https://github.com/polinadumbledore/poem_quality_evaluation/blob/main/poems_generation/poems_from_pictures_generation.ipynb) – code for generation from pictures
    * [poems_from_starters_generation.ipynb](https://github.com/polinadumbledore/poem_quality_evaluation/blob/main/poems_generation/poems_from_starters_generation.ipynb) – code for generation from starters
* [all-scripts.ipynb](https://github.com/polinadumbledore/poem_quality_evaluation/blob/main/all-scripts.ipynb) – scripts for poem evaluation on all the parameters
* [collecting_poems.ipynb](https://github.com/polinadumbledore/poem_quality_evaluation/blob/main/collecting_poems.ipynb) – code for human poems parsing
* [comparing_models.ipynb](https://github.com/polinadumbledore/poem_quality_evaluation/blob/main/comparing_models.ipynb) – code for model quality assessment
* [novelty_and_repetition.ipynb](https://github.com/polinadumbledore/poem_quality_evaluation/blob/main/novelty_and_repetition.ipynb) – code for calculating novelty and repetition 
* [rhythm.ipynb](https://github.com/polinadumbledore/poem_quality_evaluation/blob/main/rhythm.ipynb) – code for calculating rhythm
* [testing.ipynb](https://github.com/polinadumbledore/poem_quality_evaluation/blob/main/testing.ipynb) – testing of accuracy and f1-score of the model
* [training-bert.ipynb](https://github.com/polinadumbledore/poem_quality_evaluation/blob/main/training-bert.ipynb) – BERT training
* [visualisation.ipynb](https://github.com/polinadumbledore/poem_quality_evaluation/blob/main/visualisation.ipynb) – code for building graphs to describe our data

## Process
- **Collecting the Dataset (Real Poems + Generated Poems)**
  - Parsed over 1800 poems from [rustih.ru](https://rustih.ru/) and [tipoet.com](https://tipoet.com/).
  - Generated over 170 poems from various [sources](https://github.com/polinadumbledore/poem_quality_evaluation/blob/main/poems_generation/generation_sources.txt).
- **Automatic Labeling**
  - Explored various labeling algorithms and adopted best practices.
  - Customized scripts to suit our requirements.
- **Manual Labeling**
  - Defined labeling rules.
  - Conducted test-labeling and discussed the results.
  - Labeled the entire corpus.
  - Assessed the quality of labeling and resolved conflicts.
- **Training the Model**
  - Tested several models and selected the best one.
  - Conducted experiments on the results.
- **Evaluating Different Poem Generators**
- **Visualising the results**

## Parameters
- **Rhythm:** Calculated by determining the length of each line, counting the number of unique line lengths, and dividing by the total number of lines (automatically evaluated using [rhythm](https://github.com/polinadumbledore/poem_quality_evaluation/blob/main/rhythm.ipynb)).
- **Novelty:** Evaluated by comparing N-grams of a poem with N-grams of other poems to assess its uniqueness and originality (automatically assessed using [novelty & repetition](https://github.com/polinadumbledore/poem_quality_evaluation/blob/main/novelty_and_repetition.ipynb)).
- **Repetition:** Examined by comparing N-grams of a poem within itself to identify variation between lines (automatically assessed using [novelty & repetition](https://github.com/polinadumbledore/poem_quality_evaluation/blob/main/novelty_and_repetition.ipynb)).
- **Meaningfulness:** Scored based on whether more than half of the poem makes sense (1) or not (0).
- **Emotionality:** Rated as emotional (1) or not emotional (0).
- **Grammaticality:** Scored as correct (1) or containing at least one mistake (0).

### Algorithm for Manual Labeling
The corpus is divided into two halves, each labeled independently by two individuals. Before resolving conflicts, Cronbach's alpha was measured:
- Cronbach's alpha for the first pair of labelers: 0.9307134059623864
- Cronbach's alpha for the second pair of labelers: 0.9966737810000899

Conflicts were resolved by choosing 0, although consensus was largely achieved.

## Training the Model
Several models were trained to determine the best performer. Training parameters included a learning rate of 2e-5, 2 epochs comprising 9 batches, and a weight decay of 0.01. The threshold for the classes was set at 0.5.

| Model | F1 Score | Accuracy |
|-------|----------|----------|
| [rubert-base-cased with lemmatization](https://huggingface.co/DeepPavlov/rubert-base-cased) | 0.79 | 0.68 |
| [rubert-base-cased without lemmatization](https://huggingface.co/DeepPavlov/rubert-base-cased) | 0.8 | 0.69 |
| [xlm-roberta-base with lemmatization](https://huggingface.co/FacebookAI/xlm-roberta-base) | 0.8 | 0.67 |
| [xlm-roberta-base without lemmatization](https://huggingface.co/FacebookAI/xlm-roberta-base) | 0.8 | 0.67 |
| [rubert-tiny with lemmatization](https://huggingface.co/cointegrated/rubert-tiny2) | 0.8 | 0.68 |
| [rubert-tiny without lemmatization](https://huggingface.co/cointegrated/rubert-tiny2) | 0.8 | 0.67 |

The differences between models are minor. Ultimately, rubert-base-cased from DeepPavlov without lemmatization was chosen for its slightly superior performance.

The threshold of the best model was tested to determine the optimal value:

| Threshold | F1 Score | Accuracy |
|-----------|----------|----------|
| 0.5 | 0.8 | 0.69 |
| 0.6 | 0.79 | 0.7 |
| 0.7 | 0.69 | 0.62 |

A threshold of 0.6 was selected.

The accuracy of predicting each class was also assessed:

| Class | F1 Score | Accuracy |
|-------|----------|----------|
| Meaningfulness | 0.75 | 0.65 |
| Grammar | 0.81 | 0.73 |
| Emotionality | 0.661 | 0.59 |

As expected, grammar was the easiest class to predict, while emotionality had the lowest accuracy and F1 score.

The best model (rubert-base-cased without lemmatization) is avaliable on [Hugging Face](https://huggingface.co/numblilbug/rubert-cased-poem-evalutation) for public use. 

## Evaluating Different Poem Generators
Using our model, we evaluated different generative models. The score for each model was calculated by evaluating 30 poems generated by it, computing the mean value for each parameter, and then averaging all parameters:

| Model | Score |
|-------|-------|
| maxtext | 1.714286 |
| gigachat | 2.000000 |
| gpt-3.5 | 2.285714 |

## Requirements

`!pip install rouge russyl transformers`

## References
[1] M. Agirrezabal, H. G. Oliveira, A. Ormazabal. Erato: Automatizing Poetry Evaluation. 

[2] M. Elzohbi, R. Zhao. Creative Data Generation: A Review Focusing on Text and Poetry. 

[3] Automatic Generation and Evaluation of Chinese Classical Poetry with Attention-Based Deep Neural Network. 
