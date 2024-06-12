# Design Description

1) When the app is started, the user is presented with the main menu, which allows the user to (1) enter or edit current job details, (2) enter job offers, (3) adjust the comparison settings, or (4) compare job offers (disabled if no job offers were entered yet).

> The user is represented by the `User` class. The `User` class has an attribute `currentJob` to represent their current employer, as well as a private function `isEmployed()` to check if the user is currently employed. Similarly, there is a `hasOffers()` private function to determine if "compare job offers" should be shown. Job offers are stored on the `Job` class and will be associated with the user via a mapping table `UserJobMapping`. The `User` class also has a `compareJobs()` function to compare two jobs. Lastly, the `User` is also able to create comparison weights via the `ComparisonWeights` class.

2) When choosing to enter current job details, a user will

- Be shown a user interface to enter (if it is the first time) or edit all the details of their current job, which consist of:
  - Title
  - Company
  - Location (entered as city and state)
  - Cost of living in the location (expressed as an index)
  - Yearly salary
  - Yearly bonus
  - Number of stock option shares offered
  - Home Buying Program fund (one-time dollar amount up to 15% of Yearly Salary)
  - Personal Choice Holidays (A single overall number of days from 0 to 20)
  - Monthly Internet Stipend ($0 to $75 inclusive)
- Be able to either save the job details or cancel and exit without saving, returning in both cases to the main menu.

> This data is spread over a few classes. The `Job` class will store the title, location (which is a reference to a `Location` class with city/state atributes as well as other attributes commonly associated with GIS), cost of living (which is a reference to `costOfLivingIndex`, which stores relevent fields listed on the provided website), company (which is a mapping to `Company` via the `CompanyJobMapping` class), yearly salary, yearly bonus, number of stock option shares offered, home buying program fund, personal choice holidays, and monthly internet stipend. The `User` class will store the current job via the `currentJob` attribute. The `User` class will also have a `getJobOffers()` function to return an array of job offers.

3) When choosing to enter job offers, a user will

- Be shown a user interface to enter all the details of the offer, which are the same ones listed above for the current job.
- Be able to either save the job offer details or cancel.
- Be able to (1) enter another offer, (2) return to the main menu, or (3) compare the offer (if they saved it) with the current job details (if present).

> Job offeres will either link to a `Job` that was created by a `Company`, think a job listing website like Monster.com or Indeed. If that is not available a new `Job` will be created. That `Job` will be linked to a `Company` via the `CompanyJobMapping` and with the user via the `UserJobMapping`. Saving or cancelling the job offer will be a function of the UI layer. Multiple offers can be entered in the same way as previously mentioned. Comparison to another jobs, and ranking of all job offers can be done on the `User` class via the `compareJobs()` and `rankJobs()` functions, respectively.

4) When adjusting the comparison settings, the user can assign integer weights to

- Yearly salary
- Yearly bonus
- Number of Stock Option Shares Offered
- Home Buying Program Fund
- Personal Choice Holidays
- Monthly Internet Stipend

If no weights are assigned, all factors are considered equal.

> The `User` class will have a `ComparisonWeights` attribute to store these weights.

5) When choosing to compare job offers, a user will

- Be shown a list of job offers, displayed as Title and Company, ranked from best to worst (see below for details), and including the current job (if present), clearly indicated.
- Select two jobs to compare and trigger the comparison.
- Be shown a table comparing the two jobs, displaying, for each job:
  - Title
  - Company
  - Location
  - Yearly salary adjusted for cost of living
  - Yearly bonus adjusted for cost of living
  - Number of Stock Option Shares Offered
  - Home Buying Program fund (one-time up to 15% of Yearly Salary)
  - Personal Choice Holidays (A single overall number of days from 0 to 20)
  - Monthly Internet Stipend ($0 to $75 inclusive monthly)

- Be offered to perform another comparison or go back to the main menu.

> The `User` class will have a `rankJobs()` function to rank the jobs based on these weights. Job ranks are not stored to a class attribute as they will likely change constantly as new offers are entered or current offers are removed. From there the `getJobOffers()` function will allow the job offer list to be retrieved. It is the responsibility of the UI layer to display the ranked jobs in a tabular format. Similarly it is the responsibility of the UI to allow the user to restart the comparison process or navigate to the main menu.

6) When ranking jobs, a job’s score is computed as the weighted average of

AYS + AYB + (CSO/3) + HBP + (PCH *AYS / 260) + (MIS*12)

where:

- AYS = yearly salary adjusted for cost of living
- AYB = yearly bonus adjusted for cost of living
- CSO = Company shares offered (assuming a 3-year vesting schedule and a price-per-share of $1)
- HBP = Home Buying Program
- PCH = Personal Choice Holidays
- MIS= Monthly Internet Stipend

For example, if the weights are 2 for the yearly salary, 2 for the yearly bonus, 2 for Internet Stipend, and 1 for all other factors, the score would be computed as:

2/9 *AYS + 2/9* AYB + 1/9 *(CSO/3) + 1/9* HBP + 1/9 *(PCH* AYS / 260) + 2/9 *(MIS*12)

> This algorithm will be stored in logic and does not need to be stored to a class attribute. The `User` class will have a `compareJobs()` function to compare two jobs. The `User` class will also have a `rankJobs()` function to rank the jobs based on these weights.

7) The user interface must be intuitive and responsive.

> This portion is not covered here as it will be implemented in the UI layer.

8) For simplicity, you may assume there is a single system running the app (no communication or saving between devices is necessary).

> Sharding, replication, multi-node concurency, consistency, and other distributed systems concerns are not covered here as they are not part of the requirements.
