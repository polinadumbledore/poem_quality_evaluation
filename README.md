# Poem Quality Evaluation
## Main idea
The system takes a poem and evaluates it by several parameters: rhyme, rhythm, novelty, repetition, meaningfulness, emotionality, (grammaticality)

## Process
+ Collecting dataset (real poems + generated poems)
+ Automatic markup code preparation
+ Manual markup
+ Experiments with model and different sets of parameters

## Parameters
+ **rhyme**: *some description* (automatically [*reference*])
+ **rhythm**: *some description* (automatically [*reference*])
+ **novelty**: n-gramms of a poem are compared to n-gramms of other poems to evaluate poem's uniqueness, originality (automatically [*reference*])
+ **repetition**: n-gramms of a poem are compared to each other within the poem to check if there is variation between lines (automatically [*reference*])
+ **meaningfulness**: 3 - there are no mistakes considering sense, 2 - more than half of a poem makes sense, 1 - less than half of a poem makes sense, 0 - poem makes no sense
+ **emotionality**: 1 - emotional, 0 - not emotional
+ **grammaticality**: 1 - everything is correct, 0 - there is at least one mistake
#### Algorithm for manual markup:
The corpora is divided into two halves. Each half is marked up by two people independetnly. After that the results are compared. Before resolving conflcits our measure of \забыла слово типа "единство" разметки\ was equal to \здесь будет циферка\\. Then for meaningfulness we take minor value (if difference between the two is 1) or mean (if difference between the two is 2). For emotionality and grammaticality we just take the minor option.

## References
[1] *Manex Agirrezabal, Hugo Gonçalo Oliveira, Aitor Ormazabal.* Erato: Automatizing Poetry Evaluation. (2023)
