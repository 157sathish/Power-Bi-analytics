# NCR Ride Bookings -- Power BI Dashboard

## 1. Project overview

This project analyses NCR ride-booking activity for the 2024 calendar
year using **Power BI Desktop**. The source file contains 150,000
booking records and 21 columns. The Power BI report is organised into
six pages: **Home Page, Overall, Vehicle Type, Revenue, Cancellation,
and Ratings**.

The dashboard is intended to help users understand booking outcomes,
vehicle usage, booking value, ride distance, payment preferences,
cancellation patterns, and customer/driver ratings.

> **Important documentation note:** This README was prepared by
> inspecting the supplied CSV and the report layout inside the supplied
> PBIX. The PBIX layout confirms the report pages and many visual field
> assignments. It does not expose every Power Query Applied Step or the
> complete DAX definition of every model measure in a readable form.
> Therefore, the cleaning workflow below explains the appropriate
> cleaning operations for this dataset and gives you a checklist to
> compare against **Power Query Editor → Applied Steps**. It does not
> claim that an operation was applied unless it can be verified from the
> supplied files.

## 2. Files in this project

  -----------------------------------------------------------------------
  File                                Purpose
  ----------------------------------- -----------------------------------
  `ncr_ride_bookings(3).csv`          Source dataset containing
                                      ride-booking records

  `powe bi.pbix`                      Power BI report containing the data
                                      model, measures, and dashboard
                                      pages

  `README.md`                         Project documentation
  -----------------------------------------------------------------------

## 3. Dataset description

The supplied CSV has **150,000 rows and 21 columns**. The dates range
from **1 January 2024 to 30 December 2024**.

### Column dictionary

  -------------------------------------------------------------------------
  Column                                Meaning / analytical use
  ------------------------------------- -----------------------------------
  `Date`                                Date on which the booking was made;
                                        useful for daily, monthly,
                                        quarterly, and yearly trends

  `Time`                                Booking time; useful for
                                        time-of-day analysis

  `Booking ID`                          Unique booking reference (the
                                        supplied file has 148,767 distinct
                                        IDs)

  `Booking Status`                      Outcome of the booking, such as
                                        Completed, Cancelled, Incomplete,
                                        or No Driver Found

  `Customer ID`                         Customer reference used to analyse
                                        customer activity

  `Vehicle Type`                        Vehicle category, such as Auto,
                                        Bike, Go Mini, Go Sedan, Premier
                                        Sedan, eBike, and Uber XL

  `Pickup Location`                     Ride pickup area

  `Drop Location`                       Ride destination area

  `Avg VTAT`                            Average vehicle time to arrive at
                                        the pickup point

  `Avg CTAT`                            Average customer time to arrive /
                                        wait metric as defined by the
                                        source dataset

  `Cancelled Rides by Customer`         Indicator/count associated with
                                        customer-cancelled rides

  `Reason for cancelling by Customer`   Recorded reason for a customer
                                        cancellation

  `Cancelled Rides by Driver`           Indicator/count associated with
                                        driver-cancelled rides

  `Driver Cancellation Reason`          Recorded reason for a driver
                                        cancellation

  `Incomplete Rides`                    Indicator/count associated with
                                        incomplete rides

  `Incomplete Rides Reason`             Reason recorded when a ride is
                                        incomplete

  `Booking Value`                       Monetary value associated with a
                                        booking

  `Ride Distance`                       Distance associated with a ride

  `Driver Ratings`                      Rating given to the driver

  `Customer Rating`                     Rating associated with the
                                        customer/ride, as defined by the
                                        source

  `Payment Method`                      Payment type, such as UPI, Cash,
                                        Uber Wallet, Credit Card, or Debit
                                        Card
  -------------------------------------------------------------------------

## 4. Data preparation in Power Query Editor

Power Query is the ETL layer of Power BI: **Extract** data from the CSV,
**Transform** it into a reliable structure, and **Load** the prepared
table into the data model. Cleaning before building visuals prevents
incorrect sorting, misleading totals, and avoidable calculation errors.

### 4.1 Import the CSV

1.  Open Power BI Desktop.
2.  Select **Home → Get data → Text/CSV**.
3.  Choose `ncr_ride_bookings(3).csv`.
4.  Preview the columns and confirm the delimiter and headers.
5.  Select **Transform Data**, rather than immediately loading, to open
    Power Query Editor.

### 4.2 Review headers and column names

-   Confirm that the first row is being used as column headers.
-   Check for blank column names, duplicated names, accidental spaces,
    or inconsistent spelling.
-   Keep names meaningful and consistent. The report uses the table name
    `ncr_ride_bookings`.
-   Do not rename a field without checking visuals and measures that
    depend on its original name.

### 4.3 Assign correct data types

Data types are important because Power BI treats text, dates, whole
numbers, and decimals differently.

  -----------------------------------------------------------------------
  Column group                        Appropriate type
  ----------------------------------- -----------------------------------
  `Date`                              Date

  `Time`                              Time (or Text if the source time
                                      cannot be parsed consistently)

  `Booking ID`, `Customer ID`         Text

  Status, vehicle, locations,         Text
  reasons, payment method             

  VTAT, CTAT, ratings, booking value, Decimal Number
  ride distance                       

  Cancellation and incomplete         Whole Number or Decimal Number,
  indicators/counts                   depending on the source values
  -----------------------------------------------------------------------

In Power Query, select a column and use the **Data Type** menu. For
dates, use **Change Type → Using Locale** if the source date format is
ambiguous. Keep IDs as text: they are identifiers, not quantities, and
should not be summed.

### 4.4 Check missing values

The supplied source file contains missing values. The following counts
are from the CSV as uploaded:

  Column                                Missing rows
  ----------------------------------- --------------
  Avg VTAT                                    10,500
  Avg CTAT                                    48,000
  Cancelled Rides by Customer                139,500
  Reason for cancelling by Customer          139,500
  Cancelled Rides by Driver                  123,000
  Driver Cancellation Reason                 123,000
  Incomplete Rides                           141,000
  Incomplete Rides Reason                    141,000
  Booking Value                               48,000
  Ride Distance                               48,000
  Driver Ratings                              57,000
  Customer Rating                             57,000
  Payment Method                              48,000

Missing values do not all mean the same thing. For example, a blank
cancellation reason may simply mean that the ride was not cancelled. A
blank booking value may occur because a ride was not completed or no
fare was recorded. A blank rating may mean that no rating was submitted.

**Recommended treatment:** - Preserve blanks in rating, waiting-time,
fare, and distance fields when the value is genuinely unknown. Replacing
these with zero can distort averages. - For cancellation/incomplete
indicator columns, inspect the meaning of the source data before
replacing nulls. If null means "not applicable/not cancelled", replacing
null with 0 can be appropriate for count calculations---but document
this decision. - Do not replace missing reason text with a real
cancellation reason. If needed for display, use a clearly labelled
category such as `Not Applicable` or `Not Recorded`. - Compare
missingness with `Booking Status` before deciding how to treat it.

### 4.5 Check duplicates

Use **Home → Keep Rows → Keep Duplicates** or **Remove Duplicates** only
after deciding what defines a duplicate.

-   `Booking ID` is the likely business key.
-   The supplied CSV has 150,000 rows but 148,767 distinct Booking IDs,
    so there are repeated IDs in the source.
-   Do not automatically delete repeated IDs without checking whether
    they are duplicate records, multiple events, or valid repeated rows.
-   If the project rule is one row per booking, investigate the
    duplicate IDs and retain the correct record using a documented rule.

### 4.6 Check text quality and category consistency

For fields such as `Booking Status`, `Vehicle Type`, `Payment Method`,
and location names: - Apply **Transform → Format → Trim** to remove
leading/trailing spaces. - Apply **Clean** to remove non-printing
characters if present. - Check spelling and category variants before
replacing values. - Keep the original business meaning; do not combine
different vehicle types or statuses merely to make charts simpler.

### 4.7 Validate numeric ranges

Check for impossible or suspicious values: - Ratings should be within
the rating scale used by the source (commonly 1--5, but verify). -
Booking Value and Ride Distance should not be negative unless the source
has a documented adjustment/refund convention. - VTAT and CTAT should be
non-negative. - Cancellation and incomplete indicators should have the
expected values (often 0/1).

Use column profiling (**View → Column quality**, **Column
distribution**, and **Column profile**) to review valid, error, and
empty values. Profiling can be set to the top 1,000 rows or the entire
dataset; choose the entire dataset for final validation.

### 4.8 Apply and load

After checking the transformations: 1. Review the **Applied Steps** pane
and rename unclear steps. 2. Confirm there are no unexpected errors. 3.
Select **Home → Close & Apply**. 4. Wait for the data to load into the
Power BI model.

**How to document your exact cleaning:** open the PBIX → Transform data
→ select the `ncr_ride_bookings` query → copy the names and order of the
Applied Steps. Replace this section's recommended checklist with the
exact steps shown in your query (for example, `Changed Type`,
`Removed Columns`, `Replaced Value`, or `Removed Duplicates`) if those
steps are actually present.

## 5. Data modelling

Data modelling defines how data is stored, related, and calculated in
Power BI. The supplied report layout uses a table named
**`ncr_ride_bookings`**. The report appears to use this main booking
table for its visuals.

### 5.1 Column versus measure

-   A **column** stores a value for each row. Examples:
    `Booking Status`, `Vehicle Type`, and `Date`.
-   A **measure** calculates a result at query time according to the
    current filter context. Examples: total bookings, completed
    bookings, total revenue, cancellation rate, and average rating.
-   Measures are useful for KPI cards because they respond to slicers
    and filters.

### 5.2 Aggregations visible in the report

The PBIX report layout shows visuals using these aggregations: - Count
of `Booking ID` and count of `Booking Status` for booking/status
visuals. - Sum of `Booking Value` for revenue cards and payment-method
analysis. - Sum of `Ride Distance` for distance analysis. - Sum of
`Cancelled Rides by Customer` and `Cancelled Rides by Driver` for
cancellation-reason charts. - Sum of `Customer Rating` and sum of
`Driver Ratings` in rating-page cards.

**Important modelling improvement:** ratings are usually better
represented by **average rating**, not the sum of ratings. A sum can
increase simply because there are more rated rides. If the cards in the
report currently use Sum, consider changing them to Average or using
explicit average-rating measures, after confirming the intended KPI.

### 5.3 Measures: recommended definitions and explanations

The report layout exposes a custom measure/field named
`CanceledPercentage`, but the complete DAX formula is not available in
the readable report-layout metadata. The following are clear, reusable
DAX measure patterns for this dataset. Compare them with your existing
measures before adding duplicates.

``` dax
Total Bookings =
COUNTROWS ( ncr_ride_bookings )

Unique Bookings =
DISTINCTCOUNT ( ncr_ride_bookings[Booking ID] )

Completed Bookings =
CALCULATE (
    [Total Bookings],
    ncr_ride_bookings[Booking Status] = "Completed"
)

Cancelled by Customer =
CALCULATE (
    [Total Bookings],
    ncr_ride_bookings[Booking Status] = "Cancelled by Customer"
)

Cancelled by Driver =
CALCULATE (
    [Total Bookings],
    ncr_ride_bookings[Booking Status] = "Cancelled by Driver"
)

Incomplete Bookings =
CALCULATE (
    [Total Bookings],
    ncr_ride_bookings[Booking Status] = "Incomplete"
)

No Driver Found Bookings =
CALCULATE (
    [Total Bookings],
    ncr_ride_bookings[Booking Status] = "No Driver Found"
)

Total Booking Value =
SUM ( ncr_ride_bookings[Booking Value] )

Total Ride Distance =
SUM ( ncr_ride_bookings[Ride Distance] )

Average Booking Value =
AVERAGE ( ncr_ride_bookings[Booking Value] )

Average Ride Distance =
AVERAGE ( ncr_ride_bookings[Ride Distance] )

Average Customer Rating =
AVERAGE ( ncr_ride_bookings[Customer Rating] )

Average Driver Rating =
AVERAGE ( ncr_ride_bookings[Driver Ratings] )

Customer Cancellation Rate =
DIVIDE ( [Cancelled by Customer], [Total Bookings], 0 )

Driver Cancellation Rate =
DIVIDE ( [Cancelled by Driver], [Total Bookings], 0 )

Completion Rate =
DIVIDE ( [Completed Bookings], [Total Bookings], 0 )
```

#### Understanding the DAX functions

-   `COUNTROWS(table)` counts rows in the current filter context.
-   `DISTINCTCOUNT(column)` counts unique values. This is useful when
    IDs repeat.
-   `SUM(column)` adds numeric values.
-   `AVERAGE(column)` calculates the mean of non-blank numeric values.
-   `CALCULATE(expression, filter)` evaluates an expression after
    applying a filter. For example, it calculates bookings where status
    is `Completed`.
-   `DIVIDE(numerator, denominator, alternateResult)` safely divides and
    avoids a divide-by-zero error.
-   `[Measure Name]` refers to another measure. Reusing measures makes
    DAX easier to maintain.

#### How filter context works

Suppose a report user selects **Auto** in a vehicle-type slicer. A
measure such as `[Total Booking Value]` recalculates using only rows for
Auto. If the user also selects a month, the same measure is filtered by
both vehicle type and month. This is why measures are more flexible than
fixed values typed into cards.

#### Cancellation percentage: define the denominator

There are multiple valid definitions: - Customer cancellation rate =
customer-cancelled bookings / all bookings. - Driver cancellation rate =
driver-cancelled bookings / all bookings. - Total cancellation rate =
(customer-cancelled + driver-cancelled bookings) / all bookings.

Choose one definition, label the visual clearly, and avoid adding
overlapping categories twice. The custom `CanceledPercentage` visible in
the report should be checked in **Model view → select measure → formula
bar** to confirm its actual numerator and denominator.

### 5.4 Date analysis

The report uses the Date field with Power BI's Year → Quarter → Month →
Day hierarchy in trend visuals. For robust time intelligence, consider
creating a dedicated calendar table, marking it as a date table, and
relating its Date column to `ncr_ride_bookings[Date]`. This is
especially useful if you later add year-over-year or month-over-month
calculations.

## 6. Dashboard pages and visual design

The PBIX contains six report pages. The following describes the visual
fields identified in the report layout; the precise titles and
formatting should be checked in Power BI Desktop.

### 6.1 Home Page

The report contains a landing/navigation page. It provides an entry
point to the other report sections. Navigation buttons make a multi-page
report easier to use.

### 6.2 Overall

The layout contains: - A line chart with booking ID count over the Date
hierarchy (Year, Quarter, Month, Day). - A pie chart showing booking
count by `Booking Status`. - A Date slicer. - A card displaying a
booking-ID field.

**Business questions:** - How does booking volume change over time? -
What proportion of bookings fall into each status? - Are there periods
with unusual changes in booking activity?

### 6.3 Vehicle Type

The layout contains multiple cards based on the sum of `Booking Value`
and `Ride Distance`, together with a Date slicer and navigation
controls. The repeated cards are consistent with a vehicle-type
comparison page, likely using visual-level filters or buttons/bookmarks.

**Business questions:** - Which vehicle categories contribute the most
booking value? - Which categories account for the greatest ride
distance? - How does vehicle performance change when the date selection
changes?

When presenting comparisons, use consistent units and make each card's
vehicle category explicit.

### 6.4 Revenue

The layout contains: - A column chart of summed `Booking Value` by
`Payment Method`. - A column chart of summed `Ride Distance` across the
Date hierarchy. - A table containing `Customer ID` and summed
`Booking Value`. - A Date slicer.

**Business questions:** - Which payment methods account for the most
booking value? - How does ride distance change over time? - Which
customers have the highest total booking value?

The table can be sorted by booking value in descending order to show the
highest-value customers. Consider a Top N filter if the full customer
list is too long.

### 6.5 Cancellation

The layout contains: - A pie chart of customer cancellation reasons
using `Cancelled Rides by Customer`. - A pie chart of driver
cancellation reasons using `Cancelled Rides by Driver`. - A Date
slicer. - KPI cards, including a field named `CanceledPercentage`.

**Business questions:** - Why do customers cancel rides? - Why do
drivers cancel rides? - What is the cancellation rate under the selected
date filter? - Which cancellation reasons occur most often?

Use separate customer and driver cancellation visuals because their
reasons and operational causes are different.

### 6.6 Ratings

The layout contains cards using `Customer Rating` and `Driver Ratings`.

**Business questions:** - What is the average customer rating? - What is
the average driver rating? - Do rating levels change when the report is
filtered?

For meaningful rating KPIs, use Average rather than Sum, and show the
number of ratings alongside the average so that users understand the
sample size.

## 7. Slicers, filters, and report interaction

A slicer is a visual filter that lets users select a value, such as a
date range. The supplied report layout shows Date slicers on multiple
pages.

When a user changes a slicer: 1. Power BI applies the selected filter to
the relevant visuals. 2. Measures are recalculated for the filtered
rows. 3. Cards, charts, and tables update to reflect the selection.

Check **Format → Edit interactions** to confirm that each slicer filters
the intended visuals. If the report uses bookmarks or page-navigation
buttons, test every button after refreshing the data.

## 8. Key observations from the supplied source CSV

These figures are calculated from the supplied CSV file itself, not
copied from dashboard card displays.

### Booking status distribution

  Booking status                   Rows      Share
  ----------------------- ------------- ----------
  Completed                      93,000      62.0%
  Cancelled by Driver            27,000      18.0%
  No Driver Found                10,500       7.0%
  Cancelled by Customer          10,500       7.0%
  Incomplete                      9,000       6.0%
  **Total**                 **150,000**   **100%**

### Vehicle type volume and value

  Vehicle type      Booking rows   Booking value total
  --------------- -------------- ---------------------
  Auto                    37,419            12,878,422
  Go Mini                 29,806            10,338,496
  Go Sedan                27,141             9,369,719
  Bike                    22,517             7,837,697
  Premier Sedan           18,111             6,275,332
  eBike                   10,557             3,618,485
  Uber XL                  4,449             1,528,032

### Other source-level totals

-   Total booking value across non-blank values: **51,846,183**
    (currency unit is not specified in the CSV).
-   Total ride distance across non-blank values: **2,512,975.19**
    (distance unit is not specified in the CSV).
-   Average customer rating across available values: approximately
    **4.40**.
-   Average driver rating across available values: approximately
    **4.23**.
-   Distinct customer IDs: **148,788**.
-   Distinct booking IDs: **148,767**.

These totals are source-file checks. Your Power BI visuals may show
different values when slicers, filters, relationships, or measure logic
are applied.

## 9. Validation and quality checks

Before publishing, verify the following:

-   [ ] The loaded row count matches the intended source row count or
    the documented post-cleaning count.
-   [ ] Booking IDs and Customer IDs are text, not numeric measures.
-   [ ] Date is recognised as a date and sorts chronologically.
-   [ ] Null values are handled according to their business meaning.
-   [ ] Duplicate Booking IDs have been investigated.
-   [ ] Booking status categories match the source values.
-   [ ] Total booking value and ride distance reconcile with source
    totals when no filters are applied.
-   [ ] Rating cards use an appropriate aggregation.
-   [ ] Cancellation percentages have a clearly defined denominator.
-   [ ] Date slicers interact with the intended visuals.
-   [ ] All navigation buttons and bookmarks work.
-   [ ] The report refreshes without errors.

## 10. How to refresh the dashboard

1.  Keep the CSV at the expected source location, or update the source
    path in **Transform data → Data source settings**.
2.  Open the PBIX in Power BI Desktop.
3.  Select **Home → Refresh**.
4.  Check for query errors or data-type changes.
5.  Review the KPI totals and charts.
6.  Save the PBIX after confirming the report is correct.

If the CSV is moved to another computer, update the source path before
refreshing.

## 11. Suggested future improvements

-   Add a dedicated Date table and time-intelligence measures.
-   Add KPI cards for Total Bookings, Completed Bookings, Completion
    Rate, Total Booking Value, and Average Ratings.
-   Add a map or location ranking for pickup and drop areas, if the
    location data is sufficiently standardised.
-   Add a peak-hour analysis using the `Time` field.
-   Add average VTAT and CTAT KPIs, with blank values excluded from
    averages.
-   Add a Top 10 customer or location view.
-   Use average ratings rather than sums for rating KPIs.
-   Add tooltips explaining each KPI and its denominator.
-   Add a data-quality page showing missing values, duplicate IDs, and
    invalid numeric values.

## 12. Conclusion

This Power BI project turns raw NCR ride-booking records into an
interactive report covering booking trends, vehicle categories, revenue,
cancellations, and ratings. The workflow combines data preparation in
Power Query, analytical calculations in the data model, and visual
storytelling through report pages, charts, KPI cards, slicers, and
navigation.

For a portfolio presentation, be prepared to explain **why** each
transformation was performed, how a DAX measure responds to filters, why
average ratings are more meaningful than summed ratings, and how the
dashboard can help a ride-booking business make operational decisions.
