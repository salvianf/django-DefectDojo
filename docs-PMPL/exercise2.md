# ISP Documentation for get_date_range
## Code
```{python}
def get_date_range(objects):
    tz = timezone.get_current_timezone()

    start_date = objects.earliest('date').date
    start_date = datetime(start_date.year, start_date.month, start_date.day,
                        tzinfo=tz)
    end_date = objects.latest('date').date
    end_date = datetime(end_date.year, end_date.month, end_date.day,
                        tzinfo=tz)

    return start_date, end_date
```
## Input Domain Model

| Characteristics  | b1            | b2            | b3          |
|------------------|---------------|---------------|-------------|
|Object is Not Null| True          | False         |      -      |
|Start date value  | null          | < end date    | > end date  |
|end date value    | null          | < start date  | > start date|

this IDM is design by considering the object is null/not and the value of start date and end date

## IDM Relabeling Table

| Characteristics | b1  | b2  | b3  |
|-----------------|-----|-----|-----|
| A               | A1  | A2  |  -  |
| B               | B1  | B2  |  B3 |
| C               | C1  | C2  |  C3 |

## Constraints

- Month in start date and end date is in range between 1 to 12
- Day in start date and end date is depend on the months including leap year, usually it range between 1 - 31
- start date and end date is following datetime format, eg : 2021-03-29 00:00:00

## Test Values and Example I/O

Criteria Used: **Base Choice Criterion (BCC)** <br/>
This criteria is choised because there is just one teat value that can give a True and proper output, that is the base test value. The other characteristic/test value are just used to verify the implemented code is running properly. <br/>
Base Test Value : A1B2C3


| Test Value | Example Input | Expected Output |
|------------|---------------|-----------------|
| **A1B2C3** |obj_Id :{ ..., <br /> date: <br /> {start_date: "2021-03-29 00:00:00", <br /> end_date:"2021-04-12 00:00:00"} <br />}      | True, because the object exist and contain start_date and end_date which value of end_date > start_date    |
| A2B2C3     | object == null    | False , because the object not exist      |
| A1B1C3     | obj_Id :{ ..., <br /> date: <br /> {start_date: null, <br /> end_date:"2021-04-12 00:00:00"} <br />}      | False, because the start_date not exist       |
| A1B3C3     |obj_Id :{ ..., <br /> date: <br /> {start_date: "2021-05-15 00:00:00", <br /> end_date:"2021-04-12 00:00:00"} <br />}        |False, because the start_date > end_date     |
| A1B2C1     | obj_Id :{ ..., <br /> date: <br /> {start_date: "2021-03-29 00:00:00", <br /> end_date:null} <br />}         | False, because the end_date not exist      |
| A1B2C2     |obj_Id :{ ..., <br /> date: <br /> {start_date: "2021-03-29 00:00:00", <br /> end_date:"2021-02-12 00:00:00"} <br />}       | False, because the end_date < start_date       |

## Test Improvement
There are no test cases testing this functionality. For improvement, I recommend using the test values defined above to ensure the function will return the correct result ,since the function didn't have a try-except statement

