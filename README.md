# Hospital Ransomware Analysis: A Predictive Framework for Cybersecurity Risk

This project is part of the ADS-599 capstone in the Applied Data Science Program at the University of San Diego

## Project Status: Completed

## Installation

to use this project, please first clone the repo on your device using the command below:

git init

git clone https://github.com/austinmallie/ADS599_Capstone

## Project Introdution/Objective

The main purpose of this project is to provide hospitals around the US a proactive defense against cybersecurity attacks rather than reactively defend against them. The core problem is that hospital cybersecurity is fundamentally reactive. Defenses exist — but they activate after an attack has already occurred. After patients are harmed. After systems go offline. After the damage is done. Our project asks a different question: what if we could identify which hospitals are most vulnerable before an attack happens? Could the financial and operational data hospitals already report to the federal government serve as an early warning system?

The research literature paints a consistent picture. Larger hospitals face elevated breach risk due to more complex IT environments. Urban location, non-profit status, high inpatient workloads, and specialized care all correlate with higher breach probability. Financial profile matters too — profitable hospitals are attractive ransom targets, while under-resourced ones can't afford adequate protections. The most rigorous prior work, Dolezel et al. in 2023, achieved 78 to 83 percent accuracy — but was far better at identifying hospitals that weren't breached than catching the ones that were. High specificity, weak recall. That's the wrong trade-off when missing a vulnerable hospital has real patient safety consequences. No prior study produced a reproducible, longitudinal predictive framework. That gap is exactly where our project lives.

## Contributors

Austin Mallie
Cynthia Portales-Loebell
Sasha Libolt

## Methods Used
Predictive Modeling
Data Visualitization
Data Cleaning
Data Integration
Distributional analysis
Feature Engineering

## Technologies
Python 




## Project Description

Ransomware attacks on U.S. hospitals are accelerating, yet most risk frameworks remain reactive. This study develops a predictive framework using ten years of publicly available CMS cost report data, CMS Hospital General Information files, and HHS Office for Civil Rights breach records to identify hospitals at elevated cybersecurity risk. A longitudinal panel of 61,444 hospital-year observations was used to train and evaluate four binary classification models — Logistic Regression, Random Forest, Support Vector Machine, and XGBoost — using a temporal holdout strategy and cost-sensitive learning to address a severe 32:1 class imbalance. XGBoost achieved the highest performance with an AUC of 0.974 and recall of 0.884, successfully identifying 88% of breached hospitals in the 2021 holdout year. Results support the hypothesis that financial health ratios, hospital size indicators, and year-over-year trajectory variables are statistically significant predictors of cybersecurity vulnerability, enabling a reproducible, publicly available framework for proactive hospital risk assessment.

