# Poem Quality Evaluation
## Main idea
The system takes a poem and evaluates it by several parameters: rhyme, rhythm, novelty, repetition, meaningfulness, emotionality, (grammaticality)

## Process
+ Collecting dataset (real poems + generated poems)
+ Automatic markup code preparation
+ Manual labeling 
+ Experiments with model

## Parameters
+ **rhyme**: *some description* (automatically [*reference*])
+ **rhythm**: *some description* (automatically [*reference*])
+ **novelty**: n-gramms of a poem are compared to n-gramms of other poems to evaluate poem's uniqueness, originality (automatically [*reference*])
+ **repetition**: n-gramms of a poem are compared to each other within the poem to check if there is variation between lines (automatically [*reference*])
+ **meaningfulness**: 3 - there are no mistakes considering sense, 2 - more than half of a poem makes sense, 1 - less than half of a poem makes sense, 0 - poem makes no sense
+ **emotionality**: 1 - emotional, 0 - not emotional
+ **grammaticality**: 1 - everything is correct, 0 - there is at least one mistake
#### Algorithm for manual labeling:
The corpora is divided into two halves. Each half is labeled by two people independetnly and the results are compared. Before resolving conflcits we measured Cronbach's alpha:
- Cronbach's alpha for the first pair of labellers == 0.9998014298980925
- Cronbach's alpha for the second pair of labellers == 0.9996156808851054
- Cronbach's alpha for meaningfulness == 0.9975645126905592
- Cronbach's alpha for emotionality == 0.5825704412546517
- Cronbach's alpha for grammar == 0.9708282257107834

After that for meaningfulness we take minor value (if the difference between labels == 1) or mean (if the difference between labels == 2). For emotionality and grammaticality we just take the minor option.

## References
[1] *Manex Agirrezabal, Hugo Gonçalo Oliveira, Aitor Ormazabal.* Erato: Automatizing Poetry Evaluation. (2023)
