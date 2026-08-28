# Borrowing Power Calculator

## Overview

In this exercise I had to fix a simple Borrowing Power Calculator using JavaScript.

The original calculator was incomplete and used placeholder functions for calculating the tax and Household Expenditure Measure (HEM).

My main tasks were to replace the placeholders with API calls to the `server.js` to get the correct values and errors for the calculator operations.

## My Approach

First I took a look at the `borrowingCalculator.js` to see how the calculator works and where I needed to work on. I wanted to make the smallest changes to the overall code and focus on what was neccessary.

These are the changes I decided on:

1. Replace the placeholder `getTax()` function with a call to the Tax API.
2. Replaced the placeholder `getHEM()` function with a call to the HEM API.
3. Added basic error handling for unsuccessful API responses.
4. Updated the calculator to use `async`/`await` because the API requests are now asynchronous.
5. Updated existing tests to reflect the values returned by the API.
6. Added testing for HEM calculation.
7. Did manual testing.

## API Intergration

The provided `server.js` runs a local API on:

`http://localhost:3000`

The API also requires a Bearer token in the `Authorization` header.

I implemented `getTax()` and `getHEM()` using JavaScript's built-in `fetch()` function.

The Tax API is accessed using:

`GET /api/tax?income=<income>`

The API returns JSON containing the income and calculated tax.

The flow goes like this:
1. Build the API URL using the users income
2. Send a GET request using `fetch()`.
3. Include the required Bearer token.
4. Wait for API response using `await`.
5. Check resonse was successful.
6. Convert response to JSON.
7. Return the `tax` value.

The HEM API is accessed using:

`GET /api/hem?income=<income>&dependents=<dependents>`

The provided server uses income and number of dependants to determine the HEM value.

The `getHEM()` uses the same approach as `getTax()`.

### My use of `fetch()` and `async`/`await`

I used `fetch()` because the project only requires simple GET requests and `fetch()` is available in the modern version of Node.js.

I used `async`/`await` because the API requests do not return their results immediately.

The process is therefore:

- Calculate Borrowing Power

- Get Tax

- Wait for API response

- Calculate net income

- Get HEM

- Wait for API response

- Continue borrowing calculation

Using await makes this flow easier to read because the code follows the same order as the calculation.

Resources for `fetch()` and `async`/`await`:

https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch

https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/

https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/

https://www.w3schools.com/js/js_api_fetch.asp

https://www.freecodecamp.org/news/make-api-calls-in-javascript/


## Error Handling

The API can return several different error responses.

The provided server handles:

- `401 Unauthorized` - missing or invalid authentication token
- `400 Bad Request` - invalid, missing, or negative parameters
- `404 Not Found` - endpoint does not exist
- `405 Method Not Allowed` - a method other than GET was used

In `getTax()` and `getHEM()`, I checked the API response using `response.ok`.

If the API response is unsuccessful, the function throws an error rather than continuing with invalid data.

The console application catches these errors and displays an error message to the user.

This means the calculator does not continue calculating if it cannot retrieve the required Tax or HEM information.

## Testing

The project uses Mocha for testing and I had to add `async`/`await` to the tests:

Resource: https://mochajs.org/features/asynchronous-code/

Tests are run with:

`npm test`

Tests cover:

### Standard calculation

Checks that a normal set of inputs produces a positive borrowing power and the expected monthly repayment.

### Negative repayment capacity

Checks that when the user's expenses and liabilities are greater than their available income, the calculator returns zero borrowing power and zero monthly repayment.

### HEM higher than declared expenses

Checks that the calculator uses the HEM value when it is higher than the user's declared monthly expenses.

The tests help verify that the API-backed values are being used correctly by the existing borrowing power calculation.

## Running the project

1. Install dependencies: `npm install`
2. Start the API server: `npm run api` (API will run on http//:localhost:3000)
3. Open another terminal and start the calculator: `npm start`

The calculator will ask for:

- Gross Annual Income
- Number of Dependent
- Declared Monthly Expenses
- Total Credit Card Limits

The assessment rate is calculated using the baseline interest rate plus the assessment buffer.

4. Run the tests: `npm test`

### Calculation flow

1. Gets the annual tax from the API
2. Gets the HEM value from the API
3. Calculates net monthly income
4. Compares declared expenses with HEM
5. Calculates credit card liability
6. Calculates maximum monthly repayment
7. Calculates maximum borrowing power

## Summary

The main change in this project was replacing the original placeholder Tax and HEM calculations with calls to the provided API.

The existing borrowing power calculation was kept mostly unchanged.

The project uses JavaScript, fetch(), async/await, and Mocha tests to provide a simple API-backed borrowing power calculator.

AI acknowledgement: I used https://chatgpt.com/ to help with explanations, finding resources and minor adjustments to the code.