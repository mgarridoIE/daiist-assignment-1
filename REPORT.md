# Assignment 1 Report

Name: Máximo Garrido

Student ID: 18994

Email: mgarrido.ieu2022@student.ie.edu

Group: BBADBA 5B

## Dataset and purpose

I chose the Car Sale Advertisements dataset from Kaggle because I am passionate about cars and wanted to work on something useful for a car dealership. It contains 9,576 advertisements from Ukraine in 2016, with information such as brand, model, year, mileage and engine size. The target is the advertised price in US dollars.

The tool gives the dealer an initial price recommendation of the car that he is willing to sale. The employee can then adjust it according to the car and the dealership's needs. For example, they might lower the price if they want a quicker sale. However, the data does not include selling time, so the tool cannot predict whether that would work. The aim is to help with the initial estimate, while keeping the final decision with the employee.

## Data preparation

The original CSV was kept unchanged. Cleaning removed duplicates, advertisements with missing engine size or drive type, and invalid prices. Engine sizes outside 0.5–10 litres were also excluded as a practical rule, although this could remove some unusual but valid cars. Expensive cars and cars with zero mileage were not automatically removed. This left 8,364 advertisements.

Car age was calculated relative to 2016, when the data was collected. Age and mileage describe how old a car is and how much it has been used. Approximate mileage per year was added, using at least one year to avoid dividing by zero. Brand and model were combined, and uncommon categories were grouped together.

The data was split into 5,855 training rows, 1,250 validation rows and 1,259 test rows. Advertisements with identical recorded characteristics stayed in the same split. Preprocessing was learned from training data. Validation helped choose the settings, while test data was reserved for the final evaluation.

## Models and results

This is a regression problem because the output is a number: the asking price. The first comparison was a simple baseline that predicted USD 9,500 for every car. Percentage error was the main measure because the same dollar difference matters more for a cheap car than for an expensive one.

The initial linear regression had a validation percentage error of 86.60% and produced 35 negative prices. We then tried predicting the logarithm of price and converting the result back to dollars. This makes proportional price differences more comparable during training and produces positive predictions. Validation error fell to 24.95%. Three regularization settings were compared, and 0.01 gave the lowest validation percentage error. Regularization discourages excessively large weights.

The same model was implemented with scikit-learn, a manual PyTorch loop and the standard PyTorch workflow. In the manual version, the code explicitly updates the model's weights. In the standard version, an optimizer handles those updates. Both use PyTorch to calculate the gradients that guide learning.

On the test set, all three obtained a mean absolute percentage error of 25.65% and a mean absolute dollar error of USD 4,124.51. The baseline had errors of 101.70% and USD 9,845.97 respectively. The three models' predictions differed by less than one cent because they learned the same relationship from the same inputs. Their agreement checks the implementations; it does not mean that every prediction is correct. The 25.65% figure is an average, not a maximum error for each car.

Results on the same 1,259 test advertisements:

| Model | MAPE (%) | MAE (USD) |
|---|---:|---:|
| Median-price baseline | 101.70 | 9,845.97 |
| Scikit-learn | 25.65 | 4,124.51 |
| Manual PyTorch | 25.65 | 4,124.51 |
| Standard PyTorch | 25.65 | 4,124.51 |

MAPE is the mean absolute percentage error; MAE is the mean absolute error in dollars. Lower values are better.

## Dashboard

The dashboard loads the saved models without retraining them. Employees enter the car's details, see the three estimates and apply a percentage adjustment to the suggested price. Other views compare predicted and actual prices and show the distribution of prices and vehicle characteristics. Training and dashboard startup were checked locally, along with prediction and input-validation checks.

## Limitations and next steps

I would keep the current tool as a prototype. Before using it in a dealership, I would first find advertisements updated to 2026 for the relevant market and retrain the model. I would also try to reduce the error. Newer data would still need to be evaluated. Also, I would take the data from advertisements from Spain since the country from where the car is advertised conditions its price.

The dataset does not describe every car's condition, maintenance or accident history. These details can affect price even when two cars have similar recorded characteristics. The model also struggled more with cheap cars: validation percentage error was 46.08% for cars below USD 3,000. This is something that I could expect since an error of USD 500 is not the same for a USD 3,000 than for a USD 45,000 in terms of percentage.

The recommendation would remain an initial proposal rather than a final price. Errors are less consequential when an employee reviews the estimate than when prices are set automatically, but they still matter. The current results do not justify using the tool to set real prices without further work.

## Generative AI use disclosure

I used Codex to help write and debug code based on the instructions I gave throughout the project. It also helped with the dashboard and technical checks. I wrote an initial draft about the dataset and business idea, and Codex helped rewrite it more clearly. Codex also suggested technical choices and drafted explanations for the remaining report sections. These suggestions and additions he made were a draft for my review.