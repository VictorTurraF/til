# TECH6100 — Chat Transcript

Language: Australian English (en-AU)  
Prepared: 7 October 2026 (Australia/Perth)

This document translates the user prompts and visible assistant responses available in this conversation, in chronological order. Code identifiers, filenames and commands are retained; explanatory comments have been translated where needed. Progress messages are included. Tool calls, raw tool output, system/developer instructions and automatically supplied environment metadata are excluded. A file-only user message is identified explicitly. This is a historical transcript, not a corrected implementation: earlier suggestions and claims are retained even when later messages identify limitations or corrections. Dates for individual messages are not inferred.

---

## 01 — Assessment requirements and Python classes

**User**

Mate, have a look at the assessment requirements. And show me what the classes would look like in Python as well.

**Assistant — progress**

I’ll look for the assessment requirements and use them to show you what the classes would look like in Python.

**Assistant**

Mate, I read [Assessment 3](/home/victorturraflorencio/Documents/TECH6100/Assessment%2003/TECH6100%20Assessment%203.pdf). It is a **CRM for managing customers**, and the two classes you started in `main.py` are the right foundation.

**What it requires:**

| Component | Requirements |
|---|---|
| `Customer` | Integer ID, name, email, phone number, date and address; getters/setters; `__repr__`; age calculation. |
| `CustomerManager` | Customer list; add, remove, filter by age and address; export CSV or JSON. |
| Object-oriented programming | Encapsulation, inheritance and the application of good practices/design patterns. |
| Validation and exceptions | Handle invalid input and file problems with clear messages. |
| Tests | Unit tests for both classes, including error scenarios and edge cases. |
| Extensions | Add properties and methods to make the system more realistic. |
| Submission | `.py` files, in English, in Week 13. |

**Testing and exceptions together account for 20 of the 40 marks.** For the highest band, the rubric also mentions TDD and running tests from the interactive menu.

There is an ambiguity: the attribute is called `date`, but the age calculation refers to a date of birth supplied as a parameter. I would document `date` as the date of birth in `YYYY-MM-DD` format and allow it to be passed to the method.

I would organise it like this:

| Class | Responsibility |
|---|---|
| `Customer` | Represent and validate a customer. |
| `VIPCustomer(Customer)` | Demonstrate inheritance with a loyalty level, for example. |
| `CustomerManager` | Manage the collection and perform searches. |
| `CSVCustomerExporter` | Handle exporting separately from customer rules. |

A separate exporter is a design choice; **it is not a class explicitly required by the brief**.

This would be a **Python skeleton**, with the age calculation implemented and the other parts indicating what needs to be developed:

```python
from datetime import date as Date, datetime


class Customer:
    def __init__(
        self, customer_id, name, email, phone_number,
        date, address, active=True
    ):
        # Validate a positive integer ID, text fields and date of birth.
        self._id = customer_id
        self._name = name
        self._email = email
        self._phone_number = phone_number
        self._date = date
        self._address = address
        self._active = active

    def getId(self):
        return self._id

    def getName(self):
        return self._name

    def getEmail(self):
        return self._email

    def getPhoneNumber(self):
        return self._phone_number

    def getAddress(self):
        return self._address

    def getDate(self):
        return self._date

    def setName(self, newName):
        # Validate before updating.
        self._name = newName

    def setEmail(self, newEmail):
        # Validate before updating.
        self._email = newEmail

    def setPhoneNumber(self, newPhoneNumber):
        # Validate before updating.
        self._phone_number = newPhoneNumber

    def setAddress(self, newAddress):
        # Validate before updating.
        self._address = newAddress

    def calculate_age(self, date_of_birth=None, today=None):
        birth_date = datetime.strptime(
            date_of_birth if date_of_birth is not None else self._date,
            "%Y-%m-%d",
        ).date()
        today = today if today is not None else Date.today()

        if birth_date > today:
            raise ValueError("Date of birth cannot be in the future.")

        birthday_pending = (
            (today.month, today.day)
            < (birth_date.month, birth_date.day)
        )
        return today.year - birth_date.year - birthday_pending

    def deactivate(self):
        self._active = False

    def __repr__(self):
        return (
            f"{type(self).__name__}(id={self._id!r}, "
            f"name={self._name!r}, email={self._email!r}, "
            f"phone_number={self._phone_number!r}, "
            f"date={self._date!r}, address={self._address!r}, "
            f"active={self._active!r})"
        )


class VIPCustomer(Customer):
    def __init__(self, *args, loyalty_points=0, **kwargs):
        super().__init__(*args, **kwargs)
        self._loyalty_points = loyalty_points

    def add_loyalty_points(self, points):
        # Validate positive integer points before adding them.
        raise NotImplementedError

    def get_loyalty_points(self):
        return self._loyalty_points


class CSVCustomerExporter:
    def export(self, customers, filename):
        # Use csv.DictWriter, encoding="utf-8" and newline="".
        # Export fields explicitly; do not use __repr__ as CSV.
        raise NotImplementedError


class CustomerManager:
    def __init__(self, exporter=None):
        self._customers = []
        self._exporter = (
            exporter if exporter is not None
            else CSVCustomerExporter()
        )

    @property
    def customers(self):
        # A copy prevents direct changes to the internal list.
        return self._customers.copy()

    def addCustomer(self, customer: Customer):
        # Validate the type and prevent duplicate IDs.
        raise NotImplementedError

    def removeCustomer(self, customer: Customer):
        # Find by ID; handle a customer that does not exist.
        raise NotImplementedError

    def getCustomerById(self, customer_id):
        # An extra method useful for searches and updates.
        raise NotImplementedError

    def getCustomersByAgeRange(self, min_age: int, max_age: int):
        # Validate 0 <= min_age <= max_age.
        # Return customers within the range, including its boundaries.
        raise NotImplementedError

    def getCustomersByAddress(self, address: str):
        # Define and document the matching rule.
        raise NotImplementedError

    def exportData(self, filename: str):
        self._exporter.export(self._customers, filename)
```

The optional `today` parameter in the age calculation lets you test birthdays with a fixed date, without depending on the day the test runs. I kept the camelCase names specified by the assessment; for additional methods, I used snake_case.

For tests, I would mainly cover **duplicate IDs, missing customers, invalid or future dates, birthdays today/tomorrow, age-range boundaries, an empty list and export failures**.

This is still a structure to develop, not the complete program. The PDF also requires acknowledgement of AI use and a record of prompts/responses, as well as your own adaptation and understanding of the code.

## 02 — VS Code colour theme

**User**

Which VS Code colour theme would you recommend?

**Assistant — progress**

I’ll check a few theme options to recommend a good one for programming in Python.

**Assistant**

Mate, I would go with **[Catppuccin Mocha](https://github.com/catppuccin/vscode)**. It has a slightly blue dark background and pastel colours — I think it looks good and is balanced for reading Python code.

Two other options:

- **[Tokyo Night](https://marketplace.visualstudio.com/items?itemName=enkia.tokyo-night)**: a look that leans towards blue and purple; my second choice.
- **Catppuccin Latte**: if you prefer a light background, this is Catppuccin’s light variant.

**My choice for you: Mocha with a purple (`mauve`) accent**, which is already the theme’s default.

## 03 — Python unit test example

**User**

Give me an example of a unit test in Python.

**Assistant**

Mate, using your assessment: a test can check whether `calculate_age()` returns the correct age.

Create a file called **`test_customer.py`** in the same folder as `main.py`:

```python
import unittest
from datetime import date

from main import Customer


class TestCustomer(unittest.TestCase):
    def setUp(self):
        # Runs before each test.
        self.customer = Customer(
            customer_id=1,
            name="Victor",
            email="victor@example.com",
            phone_number="0400000000",
            date="2000-10-15",
            address="Perth",
        )

    def test_age_before_birthday(self):
        age = self.customer.calculate_age(
            today=date(2026, 10, 14)
        )

        self.assertEqual(age, 25)

    def test_age_on_birthday(self):
        age = self.customer.calculate_age(
            today=date(2026, 10, 15)
        )

        self.assertEqual(age, 26)

    def test_future_birth_date_raises_error(self):
        with self.assertRaises(ValueError):
            self.customer.calculate_age(
                date_of_birth="2030-01-01",
                today=date(2026, 10, 15),
            )


if __name__ == "__main__":
    unittest.main()
```

This example uses the class I showed you earlier, with the optional `today` and `date_of_birth` parameters.

- **`setUp()`** prepares a new customer for each test.
- **`assertEqual(result, expected)`** checks that the result is correct.
- **`assertRaises(ValueError)`** checks that invalid input raises the expected error.
- The methods start with **`test_`** so that `unittest` can find them.

To run it, use the terminal inside the assessment folder:

```bash
python -m unittest -v test_customer
```

If `main.py` starts a menu, put the call inside this condition so the menu does not open when the test imports `Customer`:

```python
if __name__ == "__main__":
    main()
```

A fixed date in the tests is important: **the result stays the same regardless of the day you run them.**

## 04 — Getter and setter test suites

**User**

Give me two suites, one for getters and one for setters.

**Assistant**

Mate, you can split them into two test classes: **one for getters and another for setters**. Using the `Customer` I showed you:

```python
import unittest

from main import Customer


class TestCustomerGetters(unittest.TestCase):
    def setUp(self):
        self.customer = Customer(
            customer_id=1,
            name="Victor",
            email="victor@example.com",
            phone_number="0400000000",
            date="2000-10-15",
            address="Perth",
        )

    def test_get_id(self):
        self.assertEqual(self.customer.getId(), 1)

    def test_get_name(self):
        self.assertEqual(self.customer.getName(), "Victor")

    def test_get_email(self):
        self.assertEqual(
            self.customer.getEmail(), "victor@example.com"
        )

    def test_get_phone_number(self):
        self.assertEqual(
            self.customer.getPhoneNumber(), "0400000000"
        )

    def test_get_date(self):
        self.assertEqual(self.customer.getDate(), "2000-10-15")

    def test_get_address(self):
        self.assertEqual(self.customer.getAddress(), "Perth")


class TestCustomerSetters(unittest.TestCase):
    def setUp(self):
        self.customer = Customer(
            customer_id=1,
            name="Victor",
            email="victor@example.com",
            phone_number="0400000000",
            date="2000-10-15",
            address="Perth",
        )

    def test_set_name(self):
        self.customer.setName("Ana")

        self.assertEqual(self.customer.getName(), "Ana")

    def test_set_email(self):
        self.customer.setEmail("ana@example.com")

        self.assertEqual(
            self.customer.getEmail(), "ana@example.com"
        )

    def test_set_phone_number(self):
        self.customer.setPhoneNumber("0411111111")

        self.assertEqual(
            self.customer.getPhoneNumber(), "0411111111"
        )

    def test_set_address(self):
        self.customer.setAddress("Sydney")

        self.assertEqual(self.customer.getAddress(), "Sydney")


if __name__ == "__main__":
    unittest.main()
```

Save it as `test_customer.py`. To run **everything**:

```bash
python -m unittest -v test_customer
```

To run **each group separately**:

```bash
python -m unittest -v test_customer.TestCustomerGetters
python -m unittest -v test_customer.TestCustomerSetters
```

`setUp()` creates a new customer before **each test**, so a change made in a setter test does not affect the others. These tests cover valid values; once you implement validation, add invalid-input cases.

## 05 — Age on the birthday

**User**

According to the assessment, or whatever is best to consider: if the current date is the birthday, should I display “years” or “years - 1”?

**Assistant**

On the birthday, display **`years`**, without subtracting 1. The person has already reached their new age.

For example, a date of birth of **15 October 2000**:

| Current date | Age |
|---|---:|
| 14 October 2026 | 25 |
| **15 October 2026** | **26** |
| 16 October 2026 | 26 |

The assessment asks for an age calculation without specifying a different rule. So use:

```python
years = today.year - birth_date.year

if (today.month, today.day) < (birth_date.month, birth_date.day):
    years -= 1

return years
```

Notice **`<`**, rather than `<=`: subtract one year only if the birthday **has not arrived yet**.

## 06 — Testing __repr__

**User**

How would I test the **repr** method?

**Assistant**

Call **`repr(customer)`** and compare the returned string with the expected text.

For the `__repr__` I showed earlier, the test would look like this, inside a test class that already has `setUp()` creating the customer:

```python
def test_repr(self):
    expected = (
        "Customer(id=1, name='Victor', "
        "email='victor@example.com', "
        "phone_number='0400000000', "
        "date='2000-10-15', address='Perth', "
        "active=True)"
    )

    self.assertEqual(repr(self.customer), expected)
```

**The assessment asks for detailed information, but does not define the exact format.** Adapt `expected` to the format you chose.

If you only want to check that the information appears, without requiring specific punctuation and ordering:

```python
def test_repr_contains_customer_details(self):
    result = repr(self.customer)

    for value in (
        "Victor",
        "victor@example.com",
        "0400000000",
        "2000-10-15",
        "Perth",
    ):
        with self.subTest(value=value):
            self.assertIn(value, result)
```

I would use **the first approach** if you have already defined the format: it checks the entire output, including the ID and other fields.

## 07 — Filtering a list

**User**

array filter in python, example

**Assistant**

In Python, the most common way to filter a list is a **list comprehension**:

```python
numbers = [10, 15, 20, 25, 30]

filtered = [number for number in numbers if number >= 20]

print(filtered)  # [20, 25, 30]
```

In your assessment, to filter customers by age range:

```python
def getCustomersByAgeRange(self, min_age, max_age):
    return [
        customer
        for customer in self._customers
        if min_age <= customer.calculate_age() <= max_age
    ]
```

There is also **`filter()`**, similar to JavaScript’s `.filter()`:

```python
filtered = list(filter(lambda number: number >= 20, numbers))
```

`filter()` returns an iterator; `list()` converts the result into a list. I would use the **list comprehension** for readability.

## 08 — Unit testing the manager

**User**

Give me an example of how I would test the manager class with unittest.

**Assistant — progress**

I’ll look at your `CustomerManager` so I can adapt the example to your methods.

**Assistant**

Mate, I could not access your file just now, so I will use the methods required by the assessment and the constructor we showed earlier.

The idea is to **create an empty manager before each test**, add customers and check the results of the operations:

```python
import unittest

from main import Customer, CustomerManager


class TestCustomerManager(unittest.TestCase):
    def setUp(self):
        self.manager = CustomerManager()

        self.victor = Customer(
            customer_id=1,
            name="Victor",
            email="victor@example.com",
            phone_number="0400000000",
            date="2000-10-15",
            address="Perth",
        )

        self.ana = Customer(
            customer_id=2,
            name="Ana",
            email="ana@example.com",
            phone_number="0411111111",
            date="1995-05-20",
            address="Sydney",
        )

    def test_starts_with_empty_list(self):
        self.assertEqual(self.manager.customers, [])

    def test_add_customer(self):
        self.manager.addCustomer(self.victor)

        self.assertEqual(self.manager.customers, [self.victor])

    def test_add_multiple_customers(self):
        self.manager.addCustomer(self.victor)
        self.manager.addCustomer(self.ana)

        self.assertCountEqual(
            self.manager.customers,
            [self.victor, self.ana],
        )

    def test_remove_customer_keeps_other_customers(self):
        self.manager.addCustomer(self.victor)
        self.manager.addCustomer(self.ana)

        self.manager.removeCustomer(self.victor)

        self.assertEqual(self.manager.customers, [self.ana])

    def test_get_customers_by_address(self):
        self.manager.addCustomer(self.victor)
        self.manager.addCustomer(self.ana)

        result = self.manager.getCustomersByAddress("Perth")

        self.assertEqual(result, [self.victor])

    def test_address_without_matches_returns_empty_list(self):
        self.manager.addCustomer(self.victor)

        result = self.manager.getCustomersByAddress("Melbourne")

        self.assertEqual(result, [])

    def test_filter_empty_manager_returns_empty_list(self):
        result = self.manager.getCustomersByAddress("Perth")

        self.assertEqual(result, [])


if __name__ == "__main__":
    unittest.main()
```

To test **the age filter without depending on the current date**, you can control the return value of `calculate_age()` with `patch.object`. Add this import:

```python
from unittest.mock import patch
```

And this method inside `TestCustomerManager`:

```python
def test_age_range_includes_boundaries(self):
    self.manager.addCustomer(self.victor)
    self.manager.addCustomer(self.ana)

    with (
        patch.object(self.victor, "calculate_age", return_value=25),
        patch.object(self.ana, "calculate_age", return_value=30),
    ):
        self.assertCountEqual(
            self.manager.getCustomersByAgeRange(25, 30),
            [self.victor, self.ana],
        )

        self.assertEqual(
            self.manager.getCustomersByAgeRange(26, 29),
            [],
        )
```

Here you are testing **whether the manager filters correctly**. The actual age calculation belongs in the `Customer` tests.

Save it as `test_customer_manager.py` and run:

```bash
python -m unittest -v test_customer_manager
```

This example assumes the list is available through `manager.customers`. Afterwards, add tests for **duplicate IDs, removing a missing customer, invalid age ranges and exporting**, according to the rules you implement.

## 09 — Testing while respecting encapsulation

**User**

I made customers private. How do I test it while respecting encapsulation?

**Assistant**

Test **the manager’s public behaviour**, without accessing `__customers` directly.

For example, you can use the address search to check that adding and removing work:

```python
def test_add_customer(self):
    self.manager.addCustomer(self.victor)

    result = self.manager.getCustomersByAddress("Perth")

    self.assertEqual(result, [self.victor])


def test_remove_customer(self):
    self.manager.addCustomer(self.victor)

    self.manager.removeCustomer(self.victor)

    result = self.manager.getCustomersByAddress("Perth")

    self.assertEqual(result, [])
```

If listing all customers makes sense for the system, you can also create a public method that returns **a copy of the list**:

```python
class CustomerManager:
    def __init__(self):
        self.__customers = []

    def getCustomers(self):
        return self.__customers.copy()
```

Then the test becomes:

```python
def test_add_customer(self):
    self.manager.addCustomer(self.victor)

    self.assertEqual(
        self.manager.getCustomers(),
        [self.victor],
    )
```

The copy prevents someone from calling `manager.getCustomers().clear()` and clearing the internal list. **The `Customer` objects are still shared**, but adding or removing elements in the copy does not change the manager’s collection.

I would use the existing public methods and only add `getCustomers()` if the program also needs to list all customers.

## 10 — Useful assertions for lists

**User**

Which assert methods are useful for testing lists?

**Assistant**

The most useful `unittest` methods for lists are:

| Method | What it checks |
|---|---|
| `assertEqual(actual, expected)` | The same contents **in the same order**. |
| `assertListEqual(actual, expected)` | List equality, requiring both values to be lists. |
| `assertCountEqual(actual, expected)` | The same contents and number of repetitions, **ignoring order**. |
| `assertIn(item, lista)` | The item is in the list. |
| `assertNotIn(item, lista)` | The item is not in the list. |

Examples for your manager:

```python
# Empty list
self.assertEqual(manager.getCustomers(), [])

# Number of customers
self.assertEqual(len(manager.getCustomers()), 2)

# Contents and order
self.assertListEqual(
    manager.getCustomers(),
    [victor, ana],
)

# Contents without requiring an order
self.assertCountEqual(
    manager.getCustomers(),
    [ana, victor],
)

# Added customer
self.assertIn(victor, manager.getCustomers())

# Removed customer
self.assertNotIn(victor, manager.getCustomers())
```

**Be aware of `assertCountEqual`:** it does not just check the length:

```python
self.assertCountEqual([1, 2, 2], [2, 1, 2])  # Passes
self.assertCountEqual([1, 2, 2], [1, 1, 2])  # Fails
```

For `Customer` objects, if you have not implemented `__eq__`, the default comparison uses object identity. That is why the examples work when you compare with **the same instances** that you added to the manager.

## 11 — Testing customers by age range

**User**

What would testing customers by age range look like?

**Assistant**

You can test using **controlled ages**, without accessing the private list or depending on the current date.

Add this import to your test file:

```python
from unittest.mock import patch
```

Inside `TestCustomerManager`, assuming `setUp()` creates `self.manager`, `self.victor` and `self.ana`:

```python
def test_get_customers_by_age_range(self):
    self.manager.addCustomer(self.victor)
    self.manager.addCustomer(self.ana)

    with (
        patch.object(self.victor, "calculate_age", return_value=25),
        patch.object(self.ana, "calculate_age", return_value=40),
    ):
        result = self.manager.getCustomersByAgeRange(20, 30)

    self.assertEqual(result, [self.victor])


def test_age_range_includes_min_and_max(self):
    self.manager.addCustomer(self.victor)
    self.manager.addCustomer(self.ana)

    with (
        patch.object(self.victor, "calculate_age", return_value=20),
        patch.object(self.ana, "calculate_age", return_value=30),
    ):
        result = self.manager.getCustomersByAgeRange(20, 30)

    self.assertCountEqual(result, [self.victor, self.ana])


def test_age_range_without_matches(self):
    self.manager.addCustomer(self.victor)
    self.manager.addCustomer(self.ana)

    with (
        patch.object(self.victor, "calculate_age", return_value=19),
        patch.object(self.ana, "calculate_age", return_value=31),
    ):
        result = self.manager.getCustomersByAgeRange(20, 30)

    self.assertEqual(result, [])


def test_age_range_with_empty_manager(self):
    result = self.manager.getCustomersByAgeRange(20, 30)

    self.assertEqual(result, [])
```

**`patch.object()` makes `calculate_age()` return the chosen age during the `with` block.** On leaving the block, the original method is restored.

This checks the manager’s filter: who is included, who is excluded, both boundaries and an empty list. The actual age calculation is checked separately in the `Customer` tests.

## 12 — Writing CSV files

**User**

How do I write to a CSV file?

**Assistant**

In Python, use the **`csv`** module, which is included. For your assessment, `DictWriter` lets you define columns by name:

```python
import csv

with open("customers.csv", "w", newline="", encoding="utf-8") as file:
    writer = csv.DictWriter(
        file,
        fieldnames=["id", "name", "email"],
    )

    writer.writeheader()

    writer.writerow({
        "id": 1,
        "name": "Victor",
        "email": "victor@example.com",
    })
```

This produces:

```csv
id,name,email
1,Victor,victor@example.com
```

In the **`exportData()` method of your `CustomerManager`**, it would look like this — assuming you created `getDate()`:

```python
def exportData(self, filename):
    fields = [
        "id", "name", "email",
        "phone_number", "date", "address",
    ]

    with open(filename, "w", newline="", encoding="utf-8") as file:
        writer = csv.DictWriter(file, fieldnames=fields)
        writer.writeheader()

        for customer in self.__customers:
            writer.writerow({
                "id": customer.getId(),
                "name": customer.getName(),
                "email": customer.getEmail(),
                "phone_number": customer.getPhoneNumber(),
                "date": customer.getDate(),
                "address": customer.getAddress(),
            })
```

Accessing `self.__customers` here respects encapsulation because the method belongs to the class itself.

In the interface, handle possible file errors:

```python
try:
    manager.exportData("customers.csv")
    print("Customers exported successfully.")
except OSError as error:
    print(f"Could not export customers: {error}")
```

- **`"w"`** creates the file or overwrites an existing one.
- **`newline=""`** prevents extra lines when writing CSV.
- **`encoding="utf-8"`** allows accented characters to be written.
- **`with`** closes the file automatically, even if an error occurs.

The module also handles addresses containing commas by adding quotation marks when needed.

## 13 — Inheritance ideas

**User**

Do you have any inheritance ideas I could include?

**Assistant**

Mate, for this CRM I would use **`VIPCustomer` inheriting from `Customer`**. It is easy to justify: a VIP customer is still a customer, but has additional benefits.

Some options:

| Subclass | Additional properties | Additional methods |
|---|---|---|
| `VIPCustomer` | Loyalty points | Add points, redeem points |
| `CorporateCustomer` | Company, registration number | Update company details |
| `IndividualCustomer` | Contact preference | Change preference |

**I would choose `VIPCustomer`**, because it uses all existing fields, including date of birth and age calculation.

```python
class VIPCustomer(Customer):
    def __init__(
        self, customer_id, name, email, phone_number,
        date, address, loyalty_points=0
    ):
        super().__init__(
            customer_id, name, email, phone_number,
            date, address
        )

        if type(loyalty_points) is not int:
            raise TypeError("Loyalty points must be an integer.")

        if loyalty_points < 0:
            raise ValueError("Loyalty points cannot be negative.")

        self.__loyalty_points = loyalty_points

    def getLoyaltyPoints(self):
        return self.__loyalty_points

    def addLoyaltyPoints(self, points):
        self.__validate_points(points)
        self.__loyalty_points += points

    def redeemLoyaltyPoints(self, points):
        self.__validate_points(points)

        if points > self.__loyalty_points:
            raise ValueError("Insufficient loyalty points.")

        self.__loyalty_points -= points

    @staticmethod
    def __validate_points(points):
        if type(points) is not int:
            raise TypeError("Points must be an integer.")

        if points <= 0:
            raise ValueError("Points must be greater than zero.")

    def __repr__(self):
        return (
            f"{super().__repr__()} "
            f"[loyalty_points={self.__loyalty_points}]"
        )
```

`super().__init__()` initialises common data using the `Customer` constructor. Even if those attributes are private, the subclass can use the **inherited getters and setters**:

```python
vip = VIPCustomer(
    customer_id=1,
    name="Victor",
    email="victor@example.com",
    phone_number="0400000000",
    date="2000-10-15",
    address="Perth",
)

vip.addLoyaltyPoints(100)
vip.redeemLoyaltyPoints(30)

print(vip.getName())           # Victor — inherited method
print(vip.getLoyaltyPoints())  # 70 — subclass method

manager.addCustomer(vip)
```

If the manager validates the customer’s type, use **`isinstance(customer, Customer)`**: it accepts both `Customer` and `VIPCustomer`.

This extension also gives you useful tests: adding points, redeeming points, rejecting negative values and preventing redemptions above the balance. When exporting, it is worth including the customer type and points to preserve the additional data.

## 14 — Private members

**User**

Mate, to represent private members, do you use _ or __?

**Assistant**

Mate, **both exist, but they mean different things**:

| Form | Meaning |
|---|---|
| `self.name` | Public. |
| `self._name` | Non-public **by convention**: indicates internal use, but allows direct access. |
| `self.__name` | Activates *name mangling*: Python changes the name to prevent accidental access and inheritance conflicts. |

```python
class Customer:
    def __init__(self, name):
        self.__name = name

    def getName(self):
        return self.__name
```

```python
customer = Customer("Victor")

print(customer.getName())  # Victor
print(customer.__name)     # AttributeError
```

**Python does not have completely private attributes**: `__name` can still be accessed as `_Customer__name`, but you should not use that in normal code.

For your assessment, you can keep **`__customers` and `__name`**, exposing operations through public methods. In subclasses, use the inherited getters/setters: writing `self.__name` inside `VIPCustomer` creates a different name from `Customer`’s `__name`.

## 15 — Exporting JSON

**User**

And how do I export to a JSON file?

**Assistant**

Mate, use the **`json`** module, which is included with Python. Convert the customers into a list of dictionaries and save it with `json.dump()`:

```python
import json


class CustomerManager:
    def __init__(self):
        self.__customers = []

    # Your other methods...

    def exportData(self, filename):
        data = [
            {
                "id": customer.getId(),
                "name": customer.getName(),
                "email": customer.getEmail(),
                "phone_number": customer.getPhoneNumber(),
                "date": customer.getDate(),
                "address": customer.getAddress(),
            }
            for customer in self.__customers
        ]

        with open(filename, "w", encoding="utf-8") as file:
            json.dump(data, file, indent=4, ensure_ascii=False)
```

This example assumes you created the `getDate()` getter.

To use it and handle file errors:

```python
try:
    manager.exportData("customers.json")
    print("Customers exported successfully.")
except OSError as error:
    print(f"Could not export customers: {error}")
```

The file looks like this:

```json
[
    {
        "id": 1,
        "name": "Victor",
        "email": "victor@example.com",
        "phone_number": "0400000000",
        "date": "2000-10-15",
        "address": "Perth"
    }
]
```

- **`indent=4`** formats the contents neatly.
- **`ensure_ascii=False`** keeps accented characters readable.
- **`"w"`** overwrites the file if it already exists.

You need to convert objects into dictionaries because JSON does not automatically know how to serialise a `Customer` instance.

## 16 — Validating numeric strings

**User**

How do I validate a numeric string?

**Assistant**

Mate, it depends on what you want to accept. For a string containing **only digits from `0` to `9`**, you can use:

```python
def is_numeric_string(value):
    return (
        isinstance(value, str)
        and value.isascii()
        and value.isdigit()
    )
```

```python
is_numeric_string("12345")  # True
is_numeric_string("00123")  # True
is_numeric_string("12a3")   # False
is_numeric_string("")       # False
is_numeric_string("-10")    # False
is_numeric_string("10.5")   # False
```

`isascii()` restricts the input to ASCII characters, because `isdigit()` alone also accepts some characters such as `"²"`.

**For your `Customer` phone number**, if your rule is to accept digits only:

```python
def setPhoneNumber(self, phone_number):
    if not isinstance(phone_number, str):
        raise TypeError("Phone number must be a string.")

    if not (phone_number.isascii() and phone_number.isdigit()):
        raise ValueError("Phone number must contain only digits.")

    self.__phone_number = phone_number
```

Keep phone numbers as **strings** to preserve leading zeroes. This rule rejects `+`, spaces and brackets; if you want to accept formatted numbers, you need to adjust the validation.

## 17 — Validating dates of birth

**User**

How do I validate the date of birth, using the same approach?

**Assistant**

Mate, use the same approach: check the type, the format, whether the date exists and whether it is in the future.

Assuming the format is **`YYYY-MM-DD`**:

```python
from datetime import date


def setDate(self, birth_date):
    if not isinstance(birth_date, str):
        raise TypeError("Date of birth must be a string.")

    # Require exactly YYYY-MM-DD.
    parts = birth_date.split("-")

    if (
        len(parts) != 3
        or [len(part) for part in parts] != [4, 2, 2]
        or not all(
            part.isascii() and part.isdigit()
            for part in parts
        )
    ):
        raise ValueError("Date of birth must use YYYY-MM-DD format.")

    try:
        parsed_date = date.fromisoformat(birth_date)
    except ValueError:
        raise ValueError("Date of birth must be a valid date.") from None

    if parsed_date > date.today():
        raise ValueError("Date of birth cannot be in the future.")

    self.__date = birth_date
```

Examples:

```python
customer.setDate("2000-10-15")  # Valid
customer.setDate("2000-02-29")  # Valid: leap year
customer.setDate("2001-02-29")  # Error: non-existent day
customer.setDate("2000-13-10")  # Error: non-existent month
customer.setDate("15/10/2000")  # Error: incorrect format
customer.setDate("2000-1-5")    # Error: two digits required
customer.setDate("2999-01-01")  # Error: future date
```

In the constructor, use the setter to apply the same validation during creation:

```python
self.setDate(date)
```

**A date of birth of today is allowed** — the person is zero years old. A minimum or maximum age should only be added if you define that rule for the system.

## 18 — Validating addresses

**User**

What could validate address look like?

**Assistant**

Mate, since the address is a **free-text string**, I would validate the type and prevent an empty value or whitespace-only value. I would not require numbers, because `"Perth"` can also be used in your system’s searches.

```python
def setAddress(self, address):
    if not isinstance(address, str):
        raise TypeError("Address must be a string.")

    address = address.strip()

    if not address:
        raise ValueError("Address cannot be empty.")

    self.__address = address
```

Examples:

```python
customer.setAddress("Perth")             # Valid
customer.setAddress("123 Murray Street") # Valid
customer.setAddress("  Sydney  ")        # Saves "Sydney"
customer.setAddress("")                  # ValueError
customer.setAddress("   ")               # ValueError
customer.setAddress(123)                 # TypeError
```

Reuse the validation in the constructor:

```python
self.setAddress(address)
```

This validates **that the field is filled in**, but does not confirm the address exists. For the assessment, it is a simple and justifiable initial rule.

## 19 — Testing CSV and JSON exports

**User**

Can I include a test for exporting CSV or JSON, or is that too much work for the assessment?

**Assistant**

Yes, mate — **it is worth including**, because exporting is an assessment requirement. You can use `TemporaryDirectory`: it creates a temporary folder and cleans everything up when finished.

The test should **export, open the file and check the data**. Inside `TestCustomerManager`, choose the example corresponding to the format you implemented.

Add these imports:

```python
import csv
import json
from pathlib import Path
from tempfile import TemporaryDirectory
```

**For CSV:**

```python
def test_export_csv(self):
    self.manager.addCustomer(self.victor)

    with TemporaryDirectory() as folder:
        filename = Path(folder) / "customers.csv"

        self.manager.exportData(str(filename))

        with filename.open(
            "r", newline="", encoding="utf-8"
        ) as file:
            rows = list(csv.DictReader(file))

    self.assertEqual(len(rows), 1)
    self.assertEqual(rows[0]["id"], str(self.victor.getId()))
    self.assertEqual(rows[0]["name"], self.victor.getName())
    self.assertEqual(rows[0]["email"], self.victor.getEmail())
```

**For JSON:**

```python
def test_export_json(self):
    self.manager.addCustomer(self.victor)

    with TemporaryDirectory() as folder:
        filename = Path(folder) / "customers.json"

        self.manager.exportData(str(filename))

        with filename.open("r", encoding="utf-8") as file:
            data = json.load(file)

    self.assertEqual(len(data), 1)
    self.assertEqual(data[0]["id"], self.victor.getId())
    self.assertEqual(data[0]["name"], self.victor.getName())
    self.assertEqual(data[0]["email"], self.victor.getEmail())
```

Notice that **CSV returns fields as strings**, while JSON preserves the integer ID.

These examples use the previous `setUp()` and the column names we showed. **Changing the extension does not automatically change the format**: use the test for the format your `exportData()` actually writes. The assessment allows CSV **or** JSON; you do not need to implement both.

After this basic test, the next most useful ones are exporting **an empty list** and handling **a file-writing failure**.

## 20 — Request to review Assessment 03 main.py

**User**

Can you check my Assessment 03 main.py file and tell me what I still need to add so it lines up well with what the assessment requires?

**Assistant — progress**

I’ll compare `main.py` with the assessment requirements and rubric and point out what is missing, in priority order.

**Assistant**

Mate, I tried opening it, but **the file `Assessment 03/main.py` is not available at this conversation’s path**. So I cannot yet say what is missing from your current code.

**Attach `main.py` here or paste its contents**. If your tests are separate, send those files too. I already have the PDF requirements we read and will compare them, identifying:

- What already meets the assessment requirements.
- What is missing or needs fixing, with references to the methods.
- Which tests and error handling are missing.
- Optional improvements to aim for a higher mark.

I will pay particular attention to **testing and exception handling**, which together account for **20 of the 40 marks**.

## 21 — Supplied file and review

**User**

[File-only message: `main.py` supplied at `/home/victorturraflorencio/Masters/TECH6100/Assessment 03/main.py`. No additional request text.]

**Assistant — progress**

I’ll open the file at this new path and compare the implementation with the requirements we have already read.

**Assistant — progress**

The current 20 tests passed. However, I found issues they do not yet cover: the setters bypass validation, the maximum phone-number length is not enforced, and creating a VIP with the default points raises an error. I’ll confirm these cases and separate the fixes from the requirements still missing.

**Assistant**

Mate, I was able to read [main.py](</home/victorturraflorencio/Masters/TECH6100/Assessment 03/main.py>) and run the tests: **all 20 passed**, but there are still issues and requirements to complete. I did not change the file.

You already have much of the structure: encapsulation, inheritance with `VIPCustomer`, filters, age calculation, `__repr__`, CSV/JSON export and tests that read the exported files.

**1. Fix validation first**

I confirmed these problems by running examples:

| Problem | Current behaviour | Fix |
|---|---|---|
| Setters without validation | `set_email(None)` is accepted. | Reuse the constructor’s validators. |
| Whitespace-only name/address | `"   "` is accepted and becomes `""`. | Apply `strip()` before checking length. |
| Phone-number limit | A 20-digit phone number is accepted. | Fix the length condition. |
| Incomplete email | `" @ "` is accepted and becomes `"@"`. | Validate the parts before/after `@` and prevent internal spaces. |
| VIP with default configuration | `loyalty_points=0` raises an error. | Allow an initial zero balance; require positive amounts for operations. |

For example, the [setters](</home/victorturraflorencio/Masters/TECH6100/Assessment 03/main.py:30>) should follow this pattern:

```python
def set_email(self, new_email):
    self.__email = self.__validate_email(new_email)
```

And the [phone-number validation](</home/victorturraflorencio/Masters/TECH6100/Assessment 03/main.py:74>) should be:

```python
if not 9 <= len(phone_number) <= 12:
    raise ValueError("Phone number must have between 9 and 12 digits.")
```

**2. Complete the application and error handling**

You created the `UI` utilities, but **the program currently only starts the tests**. You still need to connect a menu to the functionality:

- Register regular/VIP customers, list, update and remove them.
- Search by age and address.
- Export the data.
- Run tests from the menu — mentioned in the rubric’s highest band.

You also need to catch errors in those actions and display messages to the user. Currently, for example, an export failure propagates `OSError`; there is no interface handling it.

It is appropriate to let classes raise exceptions and catch them in the interface:

```python
try:
    manager.exportToCSV(filename)
except OSError as error:
    print(f"Could not export customers: {error}")
```

**3. Strengthen the manager’s rules**

In [CustomerManager](</home/victorturraflorencio/Masters/TECH6100/Assessment 03/main.py:202>), I would add:

| Operation | What is missing |
|---|---|
| Add | Prevent duplicate IDs — currently it even accepts the same object twice. |
| Remove | Provide a specific message when the customer does not exist. |
| Filter by age | Validate integers and `0 <= min_age <= max_age`. |
| Search by address | Validate a non-empty string — currently a whitespace search returns everyone. |

These rules are recommendations to make the CRM robust; the brief does not specify each one individually. Your partial address search is reasonable, but document that it uses partial matching and ignores case.

**4. Expand tests for errors and boundaries**

The current tests mainly cover valid operations. I would prioritise:

- **Validation:** incorrect types, whitespace, invalid email, short/long phone numbers, future dates of birth and non-existent dates.
- **Setters:** reject invalid values and preserve the previous value.
- **VIP:** initial zero balance, adding/redeeming points, insufficient balance and negative values.
- **Manager:** duplicates, removing a missing customer, an empty list, inclusive age boundaries and invalid intervals.
- **Export:** all fields, an empty list, accented characters/commas and write failures.
- **Encapsulation:** clearing the list returned by `get_customers()` must not remove the internal customers.

Your `Mock` uses `today.replace(year=...)`, which can fail on **29 February** when the target year is not a leap year. Fixed dates or controlled mocks make the tests more predictable.

**5. Align the interface with the brief**

The PDF specifies names such as `getId()`, `addCustomer()`, `getCustomersByAddress()` and `exportData()`. You used different names.

**Snake_case complies with PEP 8**, but to reduce the risk of incompatibility with marking, I would expose the required names as delegating methods:

```python
def addCustomer(self, customer):
    self.add_customer(customer)

def exportData(self, filename):
    self.exportToCSV(filename)
```

The brief also mentions date of birth as a parameter to `calculate_age()`, whereas your parameter is `today`. Since the wording is ambiguous, document that choice or allow an optional date-of-birth argument.

**6. Good-practice improvements**

- **Compatibility:** you use `Date.strptime()`, which is available in Python 3.14, the version I used to run the tests. To support earlier versions too, use `Date.fromisoformat()` for `YYYY-MM-DD` dates.
- **Design pattern:** inheritance is demonstrated, but a clearly applied design pattern is still missing. Separating CSV/JSON exporters with a common interface would be a simple application of *Strategy*.
- **VIP data:** exports do not include the customer type or points. Add these fields to preserve your extension’s data.
- **Style:** expand one-line methods, wrap long lines, remove unnecessary `pass` statements and standardise names.
- **Submission:** include the record and acknowledgement of AI use required by the PDF. TDD also requires evidence of the process; having finished tests alone does not demonstrate they were written before implementation.

**My order of work would be: fix validation and VIP → expand tests → connect the menu and handle exceptions → align names and organise exporting.** This addresses concrete problems and the areas with the greatest rubric weight first.

## 22 — Starting the UI after tests

**User**

How do I start the application/UI after the tests?

**Assistant**

Mate, `Test.main()` **exits the program by default** when it finishes. Use `exit=False` and then call your UI.

Replace the final block with something like this:

```python
def main():
    manager = CustomerManager()

    def list_customers():
        customers = manager.get_customers()

        if not customers:
            print("No customers found.")
        else:
            for customer in customers:
                print(customer)

        UI.any_key_prompt()

    UI.display_options_menu(
        "CUSTOMER MANAGEMENT SYSTEM",
        [
            (1, "List customers", list_customers),
            # Add the other actions here.
        ],
    )


if __name__ == "__main__":
    test_result = Test.main(exit=False)

    if test_result.result.wasSuccessful():
        main()
    else:
        print("Please fix the failing tests before starting the application.")
```

The flow becomes: **run tests → check whether they passed → open the menu**.

The `manager` created in `main()` starts empty: customers created by the tests belong only to the tests.

## 23 — Confirming file access

**User**

Can you see my main.py file in the folder?

**Assistant — progress**

I’ll check `main.py` in the Assessment 03 folder.

**Assistant**

Yes, mate! I can access [main.py](</home/victorturraflorencio/Masters/TECH6100/Assessment 03/main.py>) in `Masters/TECH6100/Assessment 03`.

## 24 — Adding a customer through the UI

**User**

Show me what the UI function __add_new_customer would look like.

**Assistant — progress**

I’ll look at your `UI` class and constructors to put together the function using the same pattern as your code.

**Assistant**

Mate, in your code this function belongs in the **`App` class**, because that is where `self.__manager` is held. `UI` provides the input and display methods.

You can replace the current method with this:

```python
def __add_new_customer(self):
    UI.display_header("Add a new customer")

    try:
        customer_type = UI.input_text(
            "Customer type [1 = Regular, 2 = VIP, 0 = Cancel]: "
        )

        if customer_type == "0":
            return

        if customer_type not in ("1", "2"):
            raise ValueError("Please select 1 or 2.")

        customer_id = UI.input_number("Customer ID: ")

        if any(
            customer.get_id() == customer_id
            for customer in self.__manager.get_customers()
        ):
            raise ValueError("A customer with this ID already exists.")

        name = UI.input_text("Name: ", minimum_length=3)
        email = UI.input_text("Email: ")
        phone_number = UI.input_text("Phone number (digits only): ")
        date_of_birth = UI.input_text("Date of birth (YYYY-MM-DD): ")
        address = UI.input_text("Address: ", minimum_length=3)

        customer_data = {
            "customer_id": customer_id,
            "name": name,
            "email": email,
            "phone_number": phone_number,
            "date_of_birth": date_of_birth,
            "address": address,
        }

        if customer_type == "2":
            loyalty_points = UI.input_number("Initial loyalty points: ")

            customer = VIPCustomer(
                **customer_data,
                loyalty_points=loyalty_points,
            )
        else:
            customer = Customer(**customer_data)

        self.__manager.add_customer(customer)

    except (ValueError, TypeError) as error:
        UI.any_key_prompt(f"Could not add customer: {error}")
        return

    UI.display_customers_list([customer])
    UI.any_key_prompt("Customer added successfully.")
```

**How it works:**

- All input is inside the `try`, including numeric conversions.
- The `Customer` or `VIPCustomer` constructor applies its validation.
- The customer is only added after creation completes without errors.
- `active` uses the default value of `True`.
- `**customer_data` passes the dictionary fields as constructor arguments.

I put the duplicate-ID check here to work with your current manager. Later, it would be better to move that rule to `CustomerManager.add_customer()`, ensuring validation regardless of who calls the method.

## 25 — Updating and deleting customers

**User**

What would update_customer and delete_customer look like?

**Assistant**

In your code, both methods would be in the **`App` class**, using `self.__manager`.

For **updating**, a simple option is to choose one field at a time. Your setters already perform validation:

```python
def __update_customer(self):
    UI.display_header("Update customer")

    try:
        customer_id = UI.input_number("Customer ID: ")

        customer = next(
            (
                customer
                for customer in self.__manager.get_customers()
                if customer.get_id() == customer_id
            ),
            None,
        )

        if customer is None:
            raise ValueError("Customer not found.")

        UI.display_customers_list([customer])

        print("[1] Name")
        print("[2] Email")
        print("[3] Phone number")
        print("[4] Address")
        print("[0] Cancel")

        option = UI.input_text("Select a field to update: ")

        if option == "0":
            return

        setters = {
            "1": customer.set_name,
            "2": customer.set_email,
            "3": customer.set_phone_number,
            "4": customer.set_address,
        }

        setter = setters.get(option)

        if setter is None:
            raise ValueError("Invalid option.")

        new_value = UI.input_text("New value: ")
        setter(new_value)

    except (ValueError, TypeError) as error:
        UI.any_key_prompt(f"Could not update customer: {error}")
        return

    UI.display_customers_list([customer])
    UI.any_key_prompt("Customer updated successfully.")
```

Even though `get_customers()` returns **a copy of the list**, the objects inside it are the same. Calling the setter therefore updates the customer held by the manager.

For **deletion**, search by ID and request confirmation before removing:

```python
def __delete_customer(self):
    UI.display_header("Delete customer")

    try:
        customer_id = UI.input_number("Customer ID: ")

        customer = next(
            (
                customer
                for customer in self.__manager.get_customers()
                if customer.get_id() == customer_id
            ),
            None,
        )

        if customer is None:
            raise ValueError("Customer not found.")

        UI.display_customers_list([customer])

        confirmation = UI.input_text(
            "Confirm deletion? [y/n]: "
        ).lower()

        if confirmation != "y":
            UI.any_key_prompt("Deletion cancelled.")
            return

        self.__manager.remove_customer(customer)

    except (ValueError, TypeError) as error:
        UI.any_key_prompt(f"Could not delete customer: {error}")
        return

    UI.any_key_prompt("Customer deleted successfully.")
```

Add these options to the menu in `App.main()`:

```python
(5, "Update customer", self.__update_customer),
(6, "Delete customer", self.__delete_customer),
```

`next(..., None)` returns the first customer with that ID, or `None` if none is found. Since this search is repeated, it would later be worth moving it into a public `get_customer_by_id()` method on the manager.

## 26 — Running and testing the program through the terminal

**User**

Mate, can you run the program and test it through the prompt?

**Assistant — progress**

I’ll run the program and test the menu through the terminal, including registration, updating, searching and deletion, according to the available options.

**Assistant**

Yes, mate — **I ran the program and sent input through the terminal**, going through the menus. I did not change your file.

**These worked in the tested scenarios:**

- Registering a regular customer and a VIP with zero points.
- Listing and searching by age/address.
- Updating a name.
- Cancelling and confirming deletion.
- Rejecting a duplicate ID, an invalid email during an update and an invalid date of birth.
- Messages for a missing customer and an invalid option.

The **20 unit tests also passed**.

**I found three inputs that terminate the program with an error:**

| Where | Input |
|---|---|
| Minimum age search | `abc` |
| Maximum age search | `abc` |
| Address search | ENTER only |

In the [age search](</home/victorturraflorencio/Masters/TECH6100/Assessment 03/main.py:653>), the input calls are **before the `try`**. Move them inside:

```python
def __list_customers_by_age_range(self):
    try:
        min_age = UI.input_number("Inform the minimum age: ")
        max_age = UI.input_number("Inform the maximum age: ")

        customers = self.__manager.get_customers_by_age_range(
            min_age, max_age
        )

        UI.display_header("Customers by age range")
        UI.display_customers_list(customers)

    except ValueError as error:
        print(f"Invalid age range: {error}")

    UI.any_key_prompt()
```

In the [address search](</home/victorturraflorencio/Masters/TECH6100/Assessment 03/main.py:669>), the input error is not caught:

```python
def __list_customers_by_address(self):
    try:
        address = UI.input_text(
            "Inform address to be searched: ",
            minimum_length=2,
        )

        customers = self.__manager.get_customer_by_address(address)

        if customers:
            UI.display_customers_list(customers)
        else:
            print(f"No results found for '{address}'")

    except ValueError as error:
        print(f"Invalid address: {error}")

    UI.any_key_prompt()
```

There was also a visual detail: the headings **`DATE OF BIRTHADDRESS`** run together. Increase the date-of-birth column width and add separators between columns.

Exporting passed the unit tests, but **it is not yet available in the menu** for testing through the prompt.

## 27 — UI export methods

**User**

What would the export methods look like?

**Assistant**

Mate, since your `CustomerManager` already has `exportToCSV()` and `exportToJSON()`, in the **`App` class** you only need to ask for the filename and handle errors:

```python
def __export_csv(self):
    UI.display_header("Export customers to CSV")

    try:
        filename = UI.input_text("Filename (e.g. customers.csv): ")

        if not filename.lower().endswith(".csv"):
            filename += ".csv"

        if Path(filename).exists():
            confirmation = UI.input_text(
                "File already exists. Overwrite? [y/n]: "
            ).lower()

            if confirmation != "y":
                UI.any_key_prompt("Export cancelled.")
                return

        self.__manager.exportToCSV(filename)

    except (ValueError, OSError) as error:
        UI.any_key_prompt(f"Could not export customers: {error}")
        return

    UI.any_key_prompt(
        f"Customers exported to: {Path(filename).resolve()}"
    )


def __export_json(self):
    UI.display_header("Export customers to JSON")

    try:
        filename = UI.input_text("Filename (e.g. customers.json): ")

        if not filename.lower().endswith(".json"):
            filename += ".json"

        if Path(filename).exists():
            confirmation = UI.input_text(
                "File already exists. Overwrite? [y/n]: "
            ).lower()

            if confirmation != "y":
                UI.any_key_prompt("Export cancelled.")
                return

        self.__manager.exportToJSON(filename)

    except (ValueError, OSError) as error:
        UI.any_key_prompt(f"Could not export customers: {error}")
        return

    UI.any_key_prompt(
        f"Customers exported to: {Path(filename).resolve()}"
    )
```

You already import `Path`, so you do not need another import.

In the `App.main()` menu, add:

```python
(7, "Export customers to CSV", self.__export_csv),
(8, "Export customers to JSON", self.__export_json),
```

**The UI handles interaction; the manager writes the file.** `OSError` catches problems such as missing permissions or a non-existent folder.

If you enter only `customers.csv`, the file is saved in the program’s working directory. That is why the final message displays the full path. An empty list can also be exported: a CSV with a header or JSON containing `[]`.

## 28 — Request to save this conversation

**User**

Can you save all the prompts and responses from this chat into a .md file in Australian English?

**Assistant — progress**

I’ll save a Markdown file containing the visible prompts and responses from this chat in Australian English, keeping the code examples and identifying messages that contained only a file.

**Transcript boundary**

This transcript includes the conversation through the export request and the initial acknowledgement above. Subsequent file-generation progress and the final delivery message are outside this snapshot.
