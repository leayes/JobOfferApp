# Design Description

## Assumptions and Rationale
- the data types in the diagram such as `List`, `Map` and `Location` are placeholders and not intended to be for a specific programming language
- I assume we have the cost of living data available to map between location and the cost of living index
- I treat the current job and job offers both as the `JobPosition` class, since they have the same data. The `App` has attributes for the current job and all job offers.
- The `App` class is the entrypoint to the system and initializes the various components.

## Requirement 1: When the app is started, the user is presented with the main menu, which allows the user to (1) enter or edit current job details, (2) enter job offers, (3) adjust the comparison settings, or (4) compare job offers (disabled if no job offers were entered yet).
Not represented in design, handled in GUI. The GUI may use the `mayRunComparison()` method of `App` to control whether or not the user may choose the comparison option in the UI, depending if there is enough job offers or the current job is entered yet.

## Requirement 2a: When choosing to enter current job details, a user will be shown a user interface to enter (if it is the first time) or edit all the details of their current job, which consist of: Title, Company, Location (entered as city and state), Cost of living in the location (expressed as an index), Yearly salary, Yearly bonus, Number of stock option shares offered, Home Buying Program fund (one-time dollar amount up to 15% of Yearly Salary), Personal Choice Holidays (A single overall number of days from 0 to 20), Monthly Internet Stipend ($0 to $75 inclusive)
Entering or editing the current job details will be accomplished with the constructor of the `JobPosition` class (constructors not shown in the design), and in the `update()` method. Both of these methods will check the validity of each parameter, such as making sure the Home Buying Program Fund is not above 15% of salary.

## Requirement 2b: When choosing to enter current job details, a user will be able to either save the job details or cancel and exit without saving, returning in both cases to the main menu.
Saving the current job details will call the `update()` method on `JobPosition`. Cancelling, and returning to main menu handled in the GUI.

## Requirement 3a: When choosing to enter job offers, a user will be shown a user interface to enter all the details of the offer, which are the same ones listed above for the current job.
Not represented in design, handled in GUI.

## Requirement 3b: When choosing to enter job offers, a user will be able to either save the job offer details or cancel.
Saving the details of a new job offer will call the constructor for `JobPosition`, and be added to the list of offers with the `addOffer()` method of `App`. Cancelling is handled in the GUI.

## Requirement 3c: When choosing to enter job offers, a user will be able to (1) enter another offer, (2) return to the main menu, or (3) compare the offer (if they saved it) with the current job details (if present).
All are handled in the GUI.

## Requirement 4: When adjusting the comparison settings, the user can assign integer weights to: Yearly salary, Yearly bonus, Number of Stock Option Shares Offered, Home Buying Program Fund, Personal Choice Holidays, Monthly Internet Stipend. If no weights are assigned, all factors are considered equal.
The `update()` method for `ComparisonWeights` will take the updated values from the GUI. `ComparisonWeights` has a selection of `get...()` methods that will return `1` when all weights are equal.

## Requirement 5a: When choosing to compare job offers, a user will be shown a list of job offers, displayed as Title and Company, ranked from best to worst (see below for details), and including the current job (if present), clearly indicated.
The display of the offers, as well as the current job is handle by the GUI. The ranking of the offers is returned by `App`'s `getSortedOffers()` method, which sorts each offer according to the `JobPosition`'s `score()` method, which uses the `ComparisonWeights` `get...()` methods and it's own attributes to return a score for the sorting algorithm. Sorting best to worst or worst to best may be accomplished by reversing the list if necessary, which is not shown in the diagram.

## Requirement 5b: When choosing to compare job offers, a user will select two jobs to compare and trigger the comparison.
This is handled in the GUI, see the next requirement 5c for more details that are relevant to the diagram.

## Requirement 5c: When choosing to compare job offers, a user will be shown a table comparing the two jobs, displaying, for each job: Title, Company, Location, Yearly salary adjusted for cost of living, Yearly bonus adjusted for cost of living, Number of Stock Option Shares Offered, Home Buying Program fund (one-time up to 15% of Yearly Salary), Personal Choice Holidays (A single overall number of days from 0 to 20), Monthly Internet Stipend ($0 to $75 inclusive monthly)
Since comparison is mostly showing the information of two job offers in a tabular format, most of this is handled in the GUI. The only different thing here is the use of `getAdjustedYearlySalary()` and `getAdjustedYearlyBonus()` of `JobPosition` which use the cost of living `col` of type `CostOfLiving` and the offer's `location` in order to use the `adjust()` method to calculate the cost of living adjustment.

## Requirement 5d: When choosing to compare job offers, a user will be offered to perform another comparison or go back to the main menu.
This is handled in the GUI.

## Requirement 6: When ranking jobs, a job’s score is computed as the weighted average of: AYS + AYB + (CSO/3) + HBP + (PCH \* AYS / 260) + (MIS\*12), where: AYS = yearly salary adjusted for cost of living, AYB = yearly bonus adjusted for cost of living, CSO = Company shares offered (assuming a 3-year vesting schedule and a price-per-share of $1), HBP = Home Buying Program, PCH = Personal Choice Holidays, MIS = Monthly Internet Stipend. For example, if the weights are 2 for the yearly salary, 2 for the yearly bonus, 2 for Internet Stipend, and 1 for all other factors, the score would be computed as: 2/9 \* AYS + 2/9 \* AYB + 1/9 \* (CSO/3) + 1/9 \* HBP + 1/9 \* (PCH \* AYS / 260) + 2/9 \* (MIS\*12)
This is handled by the `score()` method of `JobPosition`. It gets the weights using the `ComparisonWeights` `get...()` methods.

## Requirement 7: The user interface must be intuitive and responsive.
This is handled in the GUI.

## Requirement 8: For simplicity, you may assume there is a single system running the app (no communication or saving between devices is necessary).
There is nothing to represent in the diagram for this.
