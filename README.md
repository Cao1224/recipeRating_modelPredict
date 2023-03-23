# recipeRating_modelPredict

by Yuancheng Cao (yuc094@ucsd.edu), Grace Chen

Credit: [UC San Diego DSC 80 Winter 2023 Course Project 5 Instruction](https://dsc80.com/project5/)

## Problem Identification



## Baseline Model

**Features:**
- calories
  - 
- total fat (PDV)
  - Protein content in a recipe can be impacted by fat content because fat can mute the protein in a dish. For example, a recipe that is high in fat but low in protein will have a lower protein-to-fat ratio than a recipe that is high in protein but low in fat. The protein flavor of a dish can also be masked by fat, making it less prominent as fat can provide a more satisfying overall flavor experience.
- carbohydrates (PDV)
Note: PDV stands for “percentage of daily value”


## Final Model

Since plant-based protein may have a different nutrient profile compared to animal-based protein, this can affect the amount of protein in a recipe. I decided to add tags about plant protein and animal protein as features.

## Fairness Analysis
