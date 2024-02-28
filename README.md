<div align="center">

# SQL Challenge

[![license][license]][license-url]
[![postgres][postgres]][postgres-url]

</div>

## Description

It’s been two weeks since you were hired as a new data engineer at Pewlett Hackard (a fictional company). Your first major task is to do a research project about people whom the company employed during the 1980s and 1990s. All that remains of the employee database from that period are six CSV files.

For this project, you’ll design the tables to hold the data from the CSV files, import the CSV files into a SQL database, and then answer questions about the data. That is, you’ll perform data modeling, data engineering, and data analysis, respectively.

## Table of Contents

-   [Installation](#installation)
-   [Usage](#usage)
-   [Tests](#tests)
-   [Contributing](#contributing)
-   [License](#license)
-   [Contact](#contact)
-   [Credits](#credits)

## Installation

1.  Clone the repo

    ```sh
    git clone https://github.com/manc1n1/sql-challenge.git
    ```

2.  Change directories to `sql-challenge`

    ```sh
    cd sql-challenge
    ```

## Usage

### Data Modeling

Inspect the CSV files, and then sketch an Entity Relationship Diagram of the tables. To create the sketch, feel free to use a tool like [QuickDBD](http://www.quickdatabasediagrams.com/)

![ERD](/data/QuickDBD.png)

### Data Engineering

1. Use the provided information to create a table schema for each of the six CSV files. Be sure to do the following:

    - Remember to specify the data types, primary keys, foreign keys, and other constraints.

    - For the primary keys, verify that the column is unique. Otherwise, create a composite keyLinks to an external site., which takes two primary keys to uniquely identify a row.

    - Be sure to create the tables in the correct order to handle the foreign keys.

2. Import each CSV file into its corresponding SQL table.

    - Create the Employee Datatbase in PostgreSQL

        ```sh
        createdb employees_db
        ```

    - Connect to PostgreSQL with user `postgres`

        ```sh
        psql -U postgres
        ```

    - Connect to `employees_db`

        ```sql
        \c employees_db
        ```

    - Import data from CSV file

        ```sql
        \copy <table_name> from '</path/to/file/filename.csv>' delimiter ',' CSV HEADER;
        ```

HINT: To avoid errors, import the data in the same order as the corresponding tables got created. And, remember to account for the headers when doing the imports.

### Data Analysis

1. List the employee number, last name, first name, sex, and salary of each employee.

    ```sql
    SELECT
        e.emp_no,
        e.last_name,
        e.first_name,
        e.sex,
        s.salary
    FROM
        employees e
        LEFT JOIN salaries s ON s.emp_no = e.emp_no;
    ```

2. List the first name, last name, and hire date for the employees who were hired in 1986.

    ```sql
    SELECT
        first_name,
        last_name,
        hire_date
    FROM
        employees
    WHERE
        EXTRACT(
            YEAR
            FROM
                hire_date
        ) = 1986;
    ```

3. List the manager of each department along with their department number, department name, employee number, last name, and first name.

    ```sql
    SELECT
        d.dept_no,
        d.dept_name,
        dm.emp_no AS manager_emp_no,
        e.last_name AS manager_last_name,
        e.first_name AS manager_first_name
    FROM
        departments d
        JOIN dept_manager dm ON d.dept_no = dm.dept_no
        JOIN employees e ON dm.emp_no = e.emp_no;
    ```

4. List the department number for each employee along with that employee’s employee number, last name, first name, and department name.

    ```sql
    SELECT
        e.emp_no,
        e.last_name,
        e.first_name,
        de.dept_no,
        d.dept_name
    FROM
        employees e
        JOIN dept_emp de ON e.emp_no = de.emp_no
        JOIN departments d ON de.dept_no = d.dept_no;
    ```

5. List first name, last name, and sex of each employee whose first name is Hercules and whose last name begins with the letter B.

    ```sql
    SELECT
        first_name,
        last_name,
        sex
    FROM
        employees
    WHERE
        first_name = 'Hercules'
        AND last_name LIKE 'B%';
    ```

6. List each employee in the Sales department, including their employee number, last name, and first name.

    ```sql
    SELECT
        e.emp_no,
        e.last_name,
        e.first_name
    FROM
        employees e
        JOIN dept_emp de ON e.emp_no = de.emp_no
        JOIN departments d ON de.dept_no = d.dept_no
    WHERE
        d.dept_name = 'Sales';
    ```

7. List each employee in the Sales and Development departments, including their employee number, last name, first name, and department name.

    ```sql
    SELECT
        e.emp_no,
        e.last_name,
        e.first_name,
        d.dept_name
    FROM
        employees e
        JOIN dept_emp de ON e.emp_no = de.emp_no
        JOIN departments d ON de.dept_no = d.dept_no
    WHERE
        d.dept_name IN ('Sales', 'Development');
    ```

8. List the frequency counts, in descending order, of all the employee last names (that is, how many employees share each last name).

    ```sql
    SELECT
        last_name,
        COUNT(*) AS frequency
    FROM
        employees
    GROUP BY
        last_name
    ORDER BY
        frequency DESC,
        last_name;
    ```

## Tests

## Contributing

Contributions are what make the open source community such an amazing place to learn, inspire, and create. Any contributions you make are **greatly appreciated**.

If you have a suggestion that would make this better, please fork the repo and create a pull request. You can also simply open an issue with the tag "enhancement".
Don't forget to give the project a star! Thanks again!

1. Fork the Project
2. Create your Feature Branch (`git checkout -b feature/AmazingFeature`)
3. Commit your Changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the Branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## License

[MIT License](https://opensource.org/licenses/MIT)

## Contact

-   Email mancinij1111@gmail.com
-   GitHub [manc1n1](https://github.com/manc1n1)

## Credits

[Starter Code Download](https://static.bc-edx.com/data/dl-1-2/m9/lms/starter/Starter_Code.zip)

[license]: https://img.shields.io/github/license/manc1n1/sql-challenge.svg?style=for-the-badge
[license-url]: https://github.com/manc1n1/sql-challenge/blob/master/LICENSE
[postgres]: https://img.shields.io/badge/postgres-%23316192.svg?style=for-the-badge&logo=postgresql&logoColor=white
[postgres-url]: https://www.postgresql.org/
