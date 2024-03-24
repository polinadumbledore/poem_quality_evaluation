# Poem Quality Evaluation
## Main idea
The system takes a poem and evaluates it by several parameters: rhythm, novelty, repetition, meaningfulness, emotionality, grammar

## Process
+ Collecting dataset (real poems + generated poems)
  + parsed 1800+ poems from [rustih.ru](https://rustih.ru/) and [tipoet.com](https://tipoet.com/)
  + generated 170+ poems via different [sources](https://github.com/polinadumbledore/poem_quality_evaluation/blob/main/poems_generation/generation_sources.txt)
+ Automatic labeling
  + looked through variants of labeling algorithms, chose the best practices
  + adapted scripts to our needs
+ Manual labeling
  + thought through labeling rules
  + did test-labeling, discussed the results
  + labeled the whole corpora
  + checked quality of the labeling
  + resolved conflicts
+ Experiments with model
+ Evaluating different poem generators

## Parameters
+ **rhythm**: we calculate length of each line, count number of unique line lengths and divide it by number of lines (automatically [rhythm](https://github.com/polinadumbledore/poem_quality_evaluation/blob/main/rhythm.ipynb))
+ **novelty**: n-gramms of a poem are compared to n-gramms of other poems to evaluate poem's uniqueness, originality (automatically [novelty & repetition](https://github.com/polinadumbledore/poem_quality_evaluation/blob/main/novelty_and_repetition.ipynb))
+ **repetition**: n-gramms of a poem are compared to each other within the poem to check if there is variation between lines (automatically [novelty & repetition](https://github.com/polinadumbledore/poem_quality_evaluation/blob/main/novelty_and_repetition.ipynb))
+ **meaningfulness**: 1 - more than half of a poem makes sense, 0 - less than half of a poem makes sense
+ **emotionality**: 1 - emotional, 0 - not emotional
+ **grammaticality**: 1 - everything is correct, 0 - there is at least one mistake
### Algorithm for manual labeling:
The corpora is divided into two halves. Each half is labeled by two people independetnly and the results are compared. Before resolving conflcits we measured Cronbach's alpha:
- Cronbach's alpha for the first pair of labellers == 0.9307134059623864
- Cronbach's alpha for the second pair of labellers == 0.9966737810000899

After that we took the minor option, if there labellers scored a pom differently, though mostly we managed to be quite cohesive.

## Training the model


## Evaluating different poem generators

## References
[1] *Manex Agirrezabal, Hugo Gonçalo Oliveira, Aitor Ormazabal.* Erato: Automatizing Poetry Evaluation. (2023)
