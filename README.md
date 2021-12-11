                  #*Pewlett-Hackard-Analysis*
                  
      ##*Analysis*
     
                  
  The purpose of the analysis was to look employees at Pewlett-Hackard that are close to retirement.  The first part of the search was identifying those employees born between 1952 and 1955.  The code shown below.
*SELECT employees.emp_no,

employees.first_name,

employees.last_name,

titles.title, titles.from_date, titles.to_date

INTO retirement_titles

FROM employees

INNER JOIN titles

ON (employees.emp_no = titles.emp_no)

WHERE (birth_date BETWEEN '1952-01-01' AND '1955-12-31')

ORDER BY employees.emp_no;

An inner join was performed with the employees and titles table on the titles column.  The primary key was the emp_no.  This created the table retirement_titles with 6 columns.
Then from there we got rid of duplicate rows by using the SELECT DISTINCT ON function.  The code below created the unique_titles table that had 4 columns - emp_no, first_name, last_name, and title.

*SELECT DISTINCT ON (retirement_titles.emp_no) 

retirement_titles.emp_no,

retirement_titles.first_name,

retirement_titles.last_name,

retirement_titles.title

*INTO unique_titles

FROM retirement_titles

ORDER BY retirement_titles.emp_no, to_date DESC;

The next step was counting the number of titles held by the soon to be retiring employees.  The code below was used to accomplish that.  The SELECT COUNT function was utilized to count titles in the unique_titles and place the results into a new table called retiring_titles.  Retiring_titles contained two columns, count and titles. There were 7 different titles with varying numbers close to retirment within each title.  Screenshot below shows the table.


SELECT COUNT (unique_titles.title),

unique_titles.title

INTO retiring_titles

FROM unique_titles

GROUP BY unique_titles.title

ORDER BY unique_titles.count DESC;



![image](https://user-images.githubusercontent.com/85581208/145684904-45d9431a-57b4-4cbd-b66f-2c1657ba96bc.png)


The last part of the analysis looked at employees eligilble for mentorship.  This only looked at a birth year of 1965.  Not a very big range.  Three different tables were used to create the mentorship_eligibility table.  Two inner joins occure with the dept_emp, employees, and titles tables.  They were joined on the emp_no.

SELECT DISTINCT ON (employees.emp_no)

employees.emp_no,

employees.first_name,

employees.last_name,

employees.birth_date,

dept_emp.from_date,

dept_emp.to_date,

titles.title

INTO mentorship_eligibility

FROM employees

INNER JOIN dept_emp

	ON (dept_emp.emp_no = employees.emp_no)
	
INNER JOIN titles

	ON (titles.emp_no = employees.emp_no)
	
WHERE (dept_emp.to_date = '9999-01-01')

	AND (birth_date BETWEEN '1965-01-01' AND '1965-12-31')
	
ORDER BY employees.emp_no, employees.emp_no ASC;

##*Results

-There are 90,398 employees up for retirement in the near future.

-1549 employees were identfied for eligibility for mentorship.

-There are 7 different titles that have employees up for retirement.

-Only 2 were managers thankfully but still lots that hold senior level titles.


##*Summary

There are 90,398 employees that have been identified close to retirement.  That number was calculated in SQL with the code below.  Screenshot shows the result.


SELECT COUNT (emp_no)

FROM unique_titles;



![Screen Shot 2021-12-10 at 10 24 38 AM](https://user-images.githubusercontent.com/85581208/145607228-637533e2-f1cf-484e-b3b2-8d9cefe14451.png)


There are 1549 employees identified for mentorship.  There are plenty of senior staff to cover that group.  In fact more employees could be idenfied for mentorship and help bring up the next group. Just expand the birthdates used in the last query from just 1965 to 1965 to 1967.  Expanding the mentorship program would be very beneficial for the company.

SELECT COUNT (emp_no)

FROM mentorship_eligibility;

![Screen Shot 2021-12-10 at 10 25 11 AM](https://user-images.githubusercontent.com/85581208/145607334-384e348c-c829-497a-af8d-b08f729cd448.png)


