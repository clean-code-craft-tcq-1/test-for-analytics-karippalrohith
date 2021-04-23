# Test for Analytics

Design tests for Analytics functionality on a Battery Monitoring System.

Fill the parts marked 'enter' in the **Tasks** section below.

## Analysis-functionality to be tested

This section lists the Analysis for which _tests_ must be written.

Battery Telemetrics are collected and stored on a server.
Data for a month is stored in a [csv file](https://en.wikipedia.org/wiki/Comma-separated_values).

Analysis must be done on this csv file to find the following:
- minimum
- maximum
- count of breaches (how many times did it cross the threshold in a month?)
- record trends (date & time when the reading was continuously increasing for 30 minutes)

A PDF report of the analysis must be stored every week.
Notification must be sent when a new report is available.

## Tasks

### List Dependencies

List the dependencies of the Analysis-functionality.

1. Access to the Server containing the telemetrics in a csv file.
1. Validity of the data in csv file(NULL check, proper format, valid values in proper units).
1. Write access to server to store the PDF report.
1. Scheduling to read out the csv for analysis(continious/daily/weekly scheduling and how to determine it to trigger it again).
1. Availability of the functionality to create PDF reports from the data.
1. Availability of the functionality to connect to the mail server to send the emails.
1. Minimum threshold, Maximum threshold to find out the breaches are configured properly.
1. Email IDs to which mails need to be send are configured properly and valid.

(add more if needed)

### Mark the System Boundary

What is included in the software unit-test? What is not? Fill this table.

| Item                      | Included?     | Reasoning / Assumption
|---------------------------|---------------|---
Battery Data-accuracy       | No            | We do not test the accuracy of data
Computation of maximum      | Yes           | This is part of the software being developed
Off-the-shelf PDF converter | No			| Its a library function, need not involve in the unit test of this module. It will be covered in the unit test of the library itself.
Counting the breaches       | Yes 		 	| Part of functionality to be tested
Detecting trends            | Yes 		 	| To analyse weekly/monthly trend report
Notification utility        | Yes 		 	| To check if email was send or not whenever a new report is created.

### List the Test Cases

Write tests in the form of `<expected output or action>` from `<input>` / when `<event>`

Add to these tests:

1. Write minimum and maximum to the PDF from a csv containing positive and negative readings
1. Write "Invalid input" to the PDF when the csv doesn't contain expected data
1. Check for minimum threshold breach when readings are in the lower boundary region(If Lower Threshold =0, then check for -1,0,1)
1. Check for maximum threshold breach when readings are in the higher boundary region(If Lower Threshold =1000, then check for 99,100,101)
1. Write trend into PDF when continious 30 readings are increasing
1. Ensure trend is not recorded into PDF when continious 29 readings are increasing and the 30th reading is low compared to previous
1. Check whether the PDF report is created when data is passed to it
1. Check whether the mail is triggered when a new PDF is stored in the server
1. Check whether mail is triggered when a week/month is elapsed



(add more)

### Recognize Fakes and Reality

Consider the tests for each functionality below.
In those tests, identify inputs and outputs.
Enter one part that's real and another part that's faked/mocked.

| Functionality            | Input        | Output                      | Faked/mocked part
|--------------------------|--------------|-----------------------------|---
Read input from server     | csv file     | internal data-structure     | Fake the server store
Validate input             | csv data     | valid / invalid             | None - it's a pure function
Notify report availability | return value if report stored in server | send the mail               | Fake the sending of mail
Report inaccessible server | Status of server | Send mail             | Fake the sending of mail
Find minimum and maximum   | internal data-structure | min and max for attribute               | None - it's a pure function for min and max seperately
Detect trend               | csv data	  | date and time              | None
Write to PDF               | internal data-structure for every property of each attribute that needs to be written to pdf | pdf report creation status               | Fake the report creation
