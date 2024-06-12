# Group Design Description

## App Start

When the app is started, the user is presented with the main menu, which allows the user to (1) enter or edit current job details, (2) enter job offers, (3) adjust the comparison settings, or (4) compare job offers (disabled if no job offers were entered yet)

---

The main menu is represented by its own class with attributes storing the current job, a
list of job offers and a link to the current comparison settings. The enterCurrentJob
operation allows the user to enter or edit the current job details. The enterJobOffers
operation allows for adding job offers to the list and increments the jobOffer counter.
The adjustCompare relationship allows the user to edit the CompareSettings. Finally, the
getSortedJobOffers function allows the users to rank their current job and job offers
(if applicable) relative to their current ComparisonSettings.

## Job Details

When choosing to enter current job details, a user will:
Be shown a user interface to enter (if it is the first time) or edit all the details of
their current job, which consist of:

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

---

These details are represented by attributes in the Job class and are inherited by the
currentJob and Job offer classes.

---

Be able to either save the job details or cancel and exit without saving, returning in both cases to the main menu.

---

The save and exit relationships associated with the job class can be initiated to
perform these functions and to adjust the attributes of the MainMenu class.

## Creating Job Offers

When choosing to enter job offers, a user will:

- Be shown a user interface to enter all the details of the offer, which are the same ones
listed above for the current job.
- Be able to either save the job offer details or cancel.
- Be able to (1) enter another offer, (2) return to the main menu, or (3) compare the
offer (if they saved it) with the current job details (if applicable).

---

These details are represented by attributes in the Job class and are inherited by the
currentJob and Job offer classes. These details can be entered using the enterJobOffer
operation. The save and exit relationships associated with the job class can be
initiated to perform these functions and to adjust the attributes of the MainMenu class.
The enterJobOffer operation in the JobOffer class can be used to enter another jobOffer
before exiting.

## Job Comparisons Parameters

When adjusting the comparison settings, the user can assign integer weights to:

- Yearly salary
- Yearly bonus
- Number of Stock Option Shares Offered
- Home Buying Program Fund
- Personal Choice Holidays
- Monthly Internet Stipend

If no weights are assigned, all factors are considered equal.

---

These weights are stored in the CompareSettings class and can be adjusted through the
MainMenu class using the adjustCompare relationship.

## Comparing Jobs

When choosing to compare job offers, a user will:

- Be shown a list of job offers, displayed as Title and Company, ranked from best to
worst (see below for details), including the current job (if applicable).

> The title and company information is retrieved by the getJobData relationship form the
Job class, and the is sorted b the sortByScore operation once the scores are calculated.

- Select two jobs to compare and trigger the comparison.

> The jobs are selected once the compare operation is initiated in the MainMenu class.

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

> These details are populated into the attributes of the CompareResults class by the getJobData relationship  and are adjusted for cost of living by the adjustForCostofLiving operation from the Job class

- Be offered to perform another comparison or go back to the main menu.

> This is initiated by the anotherComparison operation in the CompatedResults class

## Job and Offer Ranking

When ranking jobs, a job’s score is computed as the weighted average of:

$$
AYS + AYB + (CSO/3) + HBP + (PCH * AYS / 260) + (MIS*12)
$$

- AYS = yearly salary adjusted for cost of living
- AYB  yearly bonus adjusted for cost of living
- CSO = Company shares offered (assuming a 3-year vesting schedule and a price-per-share of $1)
- HBP = Home Buying Program
- PCH = Personal Choice Holidays
- MIS= Monthly Internet Stipend

For example, if the weights are 2 for the yearly salary, 2 for the yearly bonus, 2 for
Internet Stipend, and 1 for all other factors, the score would be computed as:

$$
2/9 * AYS + 2/9 * AYB + 1/9 * (CSO/3) + 1/9 * HBP + 1/9 * (PCH * AYS / 260) + 2/9 * (MIS*12)
$$

---

The weighted average for each job is calculated by first using the adjustForCostofLiving
operation, and the result is used by the calcJobScore operation in the Job class. The
resulting data is exported to the ComparedResults class by the getJobData relationship.

## User Interface

The user interface must be intuitive and responsive

---

This is not represented inthe design, as it will be handled entirely within the GUI
implementation.

## App Communication

For simplicity, you may assume there is a single system running the app (no communication or saving between devices is necessary)

---

This is not represented in my design, as it will be handled entirely within the hardware
implementation.
