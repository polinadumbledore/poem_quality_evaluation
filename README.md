# Poem Quality Evaluation
## Main idea
The system takes a poem and evaluates it by several parameters: rhythm, novelty, repetition, meaningfulness, emotionality, grammar

## Process
+ Collecting dataset (real poems + generated poems)
  + parsed 1800+ poems from [stihi.ru](https://stihi.ru/)
  + generated 170+ poems via different [sources](https://github.com/polinadumbledore/poem_quality_evaluation/blob/main/poems_generation/generation_sources.txt)
+ Automatic labeling code preparation
  + looked through variants of labeling algorithms, chose the best practices
  + adapted scripts to our needs
+ Manual labeling
  + thought through labeling rules
  + did test-labeling, discussed the results
  + labeled the whole corpora
  + checked quality of the labeling
  + resolved conflicts
+ Experiments with model

## Parameters
+ **rhythm**: we calculate length of each line, count number of unique line lengths and divide it by number of lines (automatically [*reference*])
+ **novelty**: n-gramms of a poem are compared to n-gramms of other poems to evaluate poem's uniqueness, originality (automatically [novelty & repetition](https://github.com/polinadumbledore/poem_quality_evaluation/blob/main/novelty_and_repetition.ipynb))
+ **repetition**: n-gramms of a poem are compared to each other within the poem to check if there is variation between lines (automatically [novelty & repetition](https://github.com/polinadumbledore/poem_quality_evaluation/blob/main/novelty_and_repetition.ipynb))
+ **meaningfulness**: 3 - there are no mistakes considering sense, 2 - more than half of a poem makes sense, 1 - less than half of a poem makes sense, 0 - poem makes no sense
+ **emotionality**: 1 - emotional, 0 - not emotional
+ **grammaticality**: 1 - everything is correct, 0 - there is at least one mistake
### Algorithm for manual labeling:
The corpora is divided into two halves. Each half is labeled by two people independetnly and the results are compared. Before resolving conflcits we measured Cronbach's alpha:
- Cronbach's alpha for the first pair of labellers == 0.9307134059623864
- Cronbach's alpha for the second pair of labellers == 0.9966737810000899

After that for meaningfulness we take minor value (if the difference between labels == 1) or mean (if the difference between labels == 2). For emotionality and grammaticality we just take the minor option.

Just as it was expected, emotionality is quite subjective, though the Cronbach's alpha is still bigger than 0.5. As for meaningfulness and grammar, the value is very close to 1. 

## References
[1] *Manex Agirrezabal, Hugo Gonçalo Oliveira, Aitor Ormazabal.* Erato: Automatizing Poetry Evaluation. (2023)
