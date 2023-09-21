# Homework Assignment for Senior Backend/Full stack Developer (PHP)
## Objective:
Your task is to develop a loan schedule API in PHP that creates a Loan entity, calculates and returns a collection of payment segments (periods) based on the provided parameters.

Additional API method should receive adjustments to the payment schedule based on changes in Euribor interest rates.

This task involves creating a Git repository with a well-structured project, implementing the loan schedule calculation logic,
ensuring code quality, testing, error handling, and providing clear documentation.


## Requirements:

### Repository Setup:

- Provide a Git repository that we can clone.
- Ensure that we can build and run the project on a local environment using a single command (e.g., using "make" and docker-compose).
- Data persistence is not required after the container is shut down.
- You can use any preferred PHP framework, e.g., Laravel, Symfony.
- use PHP from 8.1


### API Endpoints:

1. API Endpoint Creation of Loan schedule:
Implement an API endpoint that accepts the following parameters:
- amountInCents: The principal loan amount in cents.
- term: The loan term in months.
- interestRateInBasisPoints: The initial interest rate in basis points (1 basis point = 0.01%).
- euriborRateInBasisPoints: The initial Euribor rate as a percentage.

- API Response:

The API should return a collection of payment segments similar to the provided example (below) in json.

2. API Endpoint to adjust Euribor.
   Accepts the following parameters:
- loan id
- segment Nr (recalculates rest segments as well)
- euribor in basis point


### Methodology
To calculate a loan payment schedule, you can use the annuity formula.
An annuity is a series of equal payments made at regular intervals.
In the context of a loan, it represents the fixed monthly payment that includes both principal and interest and remains
constant throughout the loan term.
The formula for calculating the monthly payment for an annuity is as follows:

`PMT = (Monthly Interest Rate * Loan Amount) / (1 - (1 + Monthly Interest Rate)^(-Number of Payments))`

* Euribor is calculated from segment's remaining principle. First segment remaining principal equals to loan initial amount.

### Example

Given:

- amount (in cents) = 1000000,
- term = 12,
- interest rate (in basis points) = 400,
- euribor default (in basis points) = 394,

When:
- Euribor not changed over time (no adjustments)

Then:

| segment # | principalPaymentInCents | interestPaymentInCents | euriborPaymentInCents | totalPaymentInCents |
|-----------|------------------------|------------------------|-----------------------|---------------------|
| 1         | 81817                  | 3333                   | 3283                  | 88433               |
| 2         | 82089                  | 3061                   | 3015                  | 88165               |
| 3         | 82363                  | 2787                   | 2745                  | 87895               |
| 4         | 82637                  | 2512                   | 2475                  | 87624               |
| 5         | 82913                  | 2237                   | 2203                  | 87353               |
| 6         | 83189                  | 1961                   | 1931                  | 87081               |
| 7         | 83467                  | 1683                   | 1658                  | 86808               |
| 8         | 83745                  | 1405                   | 1384                  | 86534               |
| 9         | 84024                  | 1126                   | 1109                  | 86259               |
| 10        | 84304                  | 846                    | 833                   | 85983               |
| 11        | 84585                  | 565                    | 556                   | 85706               |
| 12        | 84867                  | 283                    | 279                   | 85429               |

When:
- Euribor adjustment of 410 basis points applied from 6th segment (including)

Then:

| segmentNumber | principalPaymentInCents | interestPaymentInCents | euriborPaymentInCents | totalPaymentInCents |
|---------------|-------------------------|------------------------|-----------------------|---------------------|
| 1             | 81817                   | 3333                   | 3283                  | 88433               |
| 2             | 82089                   | 3061                   | 3015                  | 88165               |
| 3             | 82363                   | 2787                   | 2745                  | 87895               |
| 4             | 82637                   | 2512                   | 2475                  | 87624               |
| 5             | 82913                   | 2237                   | 2203                  | 87353               |
| 6             | 83189                   | 1961                   | 2010                  | 87160               |
| 7             | 83467                   | 1683                   | 1725                  | 86875               |
| 8             | 83745                   | 1405                   | 1440                  | 86590               |
| 9             | 84024                   | 1126                   | 1154                  | 86304               |
| 10            | 84304                   | 846                    | 867                   | 86017               |
| 11            | 84585                   | 565                    | 579                   | 85729               |
| 12            | 84867                   | 283                    | 290                   | 85440               |


You can provide interface to add multiple Euribor adjustments.

### Test Coverage:
Write tests on each use case. Loan schedule creation, Euribor payment adjustments (assume euribor changed 3-5 times a year).

### Code Quality:

1. Object-Oriented Programming (OOP):
   - Utilize Object-Oriented Programming patterns effectively in your codebase.
   
2. Code Cleanliness and Organization:
   - Ensure that the code is clean, well-organized, and adheres to best practices.

### Bonus:

- Implement Domain-Driven Design principles in your project.
- Explain, or encapsulate logic with floating point arithmetic (e.g. half round up)
- Provide a README.md in the repository with instructions on how to run the project, use the API, and any other relevant information.

### Error Handling:

Implement robust error handling for the API. Ensure that meaningful error messages are returned for invalid inputs or other issues.

### Submission:
Push your code to the provided Git repository.
Ensure that all instructions for running the project and tests are clearly documented in the README.md.
