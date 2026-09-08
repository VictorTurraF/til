# TECH6100 Assessment 2 — AI prompts and responses record

## Purpose and scope

This document records the main prompts and responses used while developing the
TECH6100 Assessment 2 bookstore application. The record is condensed for
readability but aims to preserve the substance of the collaboration, including
design decisions, Python guidance, CSV structures, menus, validation, testing,
and code changes.

Personal conversation unrelated to the assessment and internal platform
instructions are intentionally excluded. Example users and passwords mentioned
below are demonstration data only.

## Conversation record

### 1. Main menu aligned with the assessment

**Prompt:** "Quais opções colocar no menu principal, de acordo com o assessment
e mirando o HD de acordo com o marking guide?"

**Response:** Recommended a role-based menu: login/register/exit before
authentication; customer options for browsing books, filtering, cart, checkout,
and order history; staff options for book inventory, users, orders, reports, and
logout. Suggested clear navigation, validation, readable tables, and separation
of responsibilities as HD-oriented qualities.

### 2. ASCII menu layout

**Prompt:** "Só queria um layout legal ASCII para usar no menu."

**Response:** Proposed bordered ASCII menus with a title, numbered choices,
current user/role, and consistent back/logout options.

### 3. Required and optional features

**Prompt:** "Quais features o sistema deve ter? Ou está mais em aberto?"

**Response:** Separated the assessment requirements from optional enhancements.
The core included users, customers and staff, books, orders, cart/checkout,
search/filtering, inventory management, user management, order management,
reports, exception handling, persistence, and object-oriented design. Extra
features were acceptable when they supported rather than replaced the required
functionality.

### 4. `Customer` and `Admin` extending `User`

**Prompt:** "Como ficariam Customer e Admin estendendo User? O que eles teriam de
propriedades?"

**Response:** Suggested common `User` fields such as ID, name, email, password,
and role. `Customer` could add address, phone and cart. `Admin`/`Staff` could add
employee ID or access level only if useful. Recommended keeping order history in
the order repository rather than duplicating it inside the user object.

### 5. CSV persistence

**Prompt:** "Acho que vou utilizar CSV para armazenar os dados."

**Response:** Supported the decision and proposed three files: `users.csv`,
`books.csv`, and `orders.csv`. Recommended repositories to isolate file access,
using `csv.DictReader` and `csv.DictWriter`, stable column headers, conversions at
the repository boundary, and safe rewriting for updates/removals.

### 6. Useful PyCharm shortcuts

**Prompt:** "Quais keyboard shortcuts do PyCharm são mais úteis para mim, que já
sou experiente com programming?"

**Response:** Listed high-value navigation, refactoring, generation, search,
formatting, run/debug, selection, and multi-caret shortcuts, prioritising actions
such as Search Everywhere, Find Action, Rename, Extract Method, Go to
Declaration, usages, reformat, and run/debug.

### 7. Repository architecture

**Prompt:** "Vou fazer users, books and orders CSV repositories. Qual o melhor
design? Posso utilizar abstração e herança?"

**Response:** Recommended repository interfaces plus CSV implementations, with a
small reusable `CSVRepository` base class for file mechanics only. Domain-specific
operations should remain in `UserCSVRepository`, `BookCSVRepository`, and
`OrderCSVRepository`. This demonstrated abstraction and inheritance without
creating an overly complex generic framework.

### 8. Interfaces and implementations

**Prompt:** "Vou fazer as interfaces em impl para os repos."

**Response:** Proposed interfaces using `ABC` and `@abstractmethod`, then concrete
CSV implementations. Example layout: repository interfaces, a shared CSV base,
and concrete implementations. Naming such as `BookRepository` and
`CSVBookRepository` was suggested as an alternative to an `impl` folder.

### 9. Multiple inheritance

**Prompt:** "No Python dá para estender CSVRepository e implementar
BookRepositoryInterface?"

**Response:** Yes. Demonstrated:

```python
class CSVBookRepository(CSVRepository, BookRepositoryInterface):
    def find_all(self):
        ...
```

Explained that every abstract method must be implemented before the concrete
class can be instantiated.

### 10. Inline abstract method syntax

**Prompt:** "Dá para usar o decorator inline, tipo `@abstractmethod def meth():
pass`?"

**Response:** The decorator must be placed on the line immediately above the
method:

```python
@abstractmethod
def method(self):
    pass
```

Using `...` instead of `pass` is also valid.

### 11. CSV structures

**Prompt:** "Recapitula a estrutura do CSV."

**Response:** Proposed schemas similar to:

```text
users.csv: id,name,email,password,role,address,phone,employee_id
books.csv: id,title,author,book_type,price,stock_quantity
orders.csv: id,customer_id,items,total,status,created_at
```

For order items, JSON text inside one CSV cell was suggested when an additional
`order_items.csv` file was outside the chosen scope.

### 12. Python `next()`

**Prompt:** "Que método é este `next()`?"

**Response:** Explained that `next(iterator, default)` retrieves the next item
from an iterator. With a generator expression, it is useful for returning the
first matching object without building a complete list:

```python
book = next((book for book in books if book.id == book_id), None)
```

### 13. Checking customer/admin types

**Prompt:** "Como checar `instanceof` Admin/Customer? `user is Admin` funciona?"

**Response:** Use `isinstance(user, Admin)` or `isinstance(user, Customer)`.
`user is Admin` compares the object identity of an instance with the class object
and therefore does not perform a type check.

### 14. Passing rows to `_write_rows()`

**Prompt:** "Para escrever utilizando `_write_rows()`, como passo? Dict?"

**Response:** Recommended a list of dictionaries when using `DictWriter`:

```python
rows = [book.to_dict() for book in books]
self._write_rows(rows)
```

Each dictionary key should match a CSV field name. For one new row, read the
current rows, append the dictionary, and rewrite, or provide a dedicated
append-row helper.

### 15. Generating IDs

**Prompt:** "Como gerar um ID randômico?"

**Response:** Recommended UUID-derived IDs with a readable prefix:

```python
from uuid import uuid4

user_id = f"U-{uuid4().hex[:8].upper()}"
```

Also recommended checking uniqueness against repository data when truncating a
UUID.

### 16. Private passwords and CSV

**Prompt:** "Se password é privado na classe User, como passo ele para adicionar
no CSV?"

**Response:** Encapsulation does not prevent controlled persistence. Suggested a
read-only property or a `to_dict()` method that returns the stored password hash,
not the original plain-text password. The repository can call this public
serialization method without accessing name-mangled attributes.

### 17. Password hashing

**Prompt:** "Qual função hash posso usar sem comprometer o marking guide e o
assessment outline?"

**Response:** Suggested PBKDF2-HMAC-SHA256 from Python's standard library, with a
unique random salt and many iterations. This avoided external dependencies while
demonstrating secure handling. Password verification should recompute the hash
with the saved salt and compare using `hmac.compare_digest`.

### 18. Adding to a dictionary

**Prompt:** "Como fazer um append em um dictionary?"

**Response:** Dictionaries do not have `append`; assign a key/value pair:

```python
data["status"] = "pending"
```

If the value is a list, append to that list:

```python
data.setdefault("items", []).append(item)
```

### 19. Books CSV and sample books

**Prompts:** "Recapitula as colunas do books.csv" and "Gera o arquivo books.csv
com 10 livros."

**Response:** Used columns for book ID, title, author, type/category, price and
stock quantity, then generated ten varied sample records suitable for search,
filtering, sorting, checkout, and low-stock testing.

### 20. Terminal table formatting

**Prompt:** "Como fazer uma tabulação de tabela em Python no terminal?"

**Response:** Showed fixed-width formatted strings, for example:

```python
print(f"{'ID':<12} {'Title':<30} {'Price':>10} {'Stock':>8}")
print(f"{book.id:<12} {book.title:<30} {book.price:>10.2f} "
      f"{book.stock_quantity:>8}")
```

This keeps the project dependency-free and provides readable output.

### 21. Orders CSV test data

**Prompt:** "Gera o orders.csv com alguns pedidos de teste. Pode utilizar este
user: Ronald..."

**Response:** Generated test orders linked to the provided demonstration customer,
using valid book IDs, totals, statuses, dates, and serialised line items.

### 22. Getters/setters and listing orders

**Prompt:** "Devo respeitar os getters e setters especificados no assessment
outline ou posso acrescentar/mudar conforme necessário? Estou pensando em como
listar todos os pedidos."

**Response:** Recommended retaining the explicitly requested public methods and
adding sensible methods where required. Listing all orders is a repository
responsibility (`find_all()`), so it does not require exposing an entity's private
attributes or adding unrelated setters.

### 23. String cleanup and substring search

**Prompts:** "O que `str.strip()` faz?" and "Como faz um `str.contains('sub')`?"

**Response:** `strip()` removes leading and trailing whitespace and returns a new
string. For case-insensitive substring checks in plain Python:

```python
if search_term.strip().casefold() in book.title.casefold():
    ...
```

### 24. Filter by book type

**Prompt:** "Qual a ideia por trás do filter by book type? É para mostrar um
dropdown ou pegar um search term do usuário?"

**Response:** For a terminal UI, recommended a numbered list of valid book types
instead of arbitrary text. The user then chooses one exact category. Keeping a
fixed `BOOK_TYPES` tuple was accepted as appropriate for this assessment.

### 25. Sorting lists

**Prompts:** "Como fazer um array.sort no Python?" and "O sort aplica as
alterações na instância?"

**Response:** `list.sort()` modifies the same list and returns `None`; `sorted()`
returns a new list. Example:

```python
books.sort(key=lambda book: book.title.casefold())
ordered_books = sorted(books, key=lambda book: book.price)
```

### 26. Shopping cart menu

**Prompt:** "Recapitula as opções do shopping cart do cliente."

**Response:** Recommended: view cart, add book, update quantity, remove item,
checkout, clear cart, and back. Stock checks and totals should occur during cart
operations and again during checkout.

### 27. Add book to cart and current customer

**Prompts:** "Como ficaria a função add book to cart?" and "De onde vou pegar o
customer?"

**Response:** The logged-in customer should come from application/session state,
not be recreated from input. The add operation finds the book, validates stock
and quantity, then updates an existing cart item or adds a new one. The menu
should only expose customer operations after a `Customer` successfully logs in.

### 28. Application/session state

**Prompt:** "Vou fazer uma classe estática para as variáveis do sistema, para
guardar o usuário logado."

**Response:** Accepted a small session class with a class attribute for the
current user and `login`, `logout`, and `require_customer`/`require_admin`
operations. Warned against turning it into a global container for repositories
and unrelated state.

### 29. View, update and remove cart items

**Prompts:** "Como ficaria o view cart?", "Como ficaria o update quantity?" and
"Como ficaria o remover do carrinho?"

**Response:** `view_cart` should display each line's book, quantity, unit price,
subtotal and grand total. Updating quantity must validate positive integers and
available stock; zero can either be rejected or explicitly treated as removal.
Removing should locate the cart item by book ID and raise a friendly error when
not found.

### 30. Custom exceptions

**Prompt:** "No except, ele mostra a mensagem de erro customizada?"

**Response:** Yes, when the exception is raised with a message and printed as the
caught exception:

```python
try:
    ...
except ValueError as error:
    print(f"Error: {error}")
```

### 31. Checkout

**Prompt:** "Legal, partiu checkout."

**Response:** Proposed validating that the cart is not empty, rechecking every
book and stock quantity, calculating the total, creating an order, reducing book
stock, saving books and the order, then clearing the cart only after persistence
succeeds. A status such as `Pending` was recommended for new orders.

### 32. Repository add/update methods

**Prompt:** "Preciso implementar o add e update dos repos."

**Response:** `add()` should reject duplicate IDs, serialise the entity and
append/rewrite safely. `update()` should read all rows, replace the row with the
matching ID, raise a not-found exception if absent, and write the complete result
back. Domain-to-row and row-to-domain conversions should stay in the concrete
repository.

### 33. Customer order history

**Prompt:** "Bora implementar o my_orders."

**Response:** Get the current customer from the session, call an order repository
method such as `find_by_customer_id(customer.id)`, then display the matching
orders and items. A clear "no orders found" message should be shown for an empty
result.

### 34. Staff add book

**Prompt:** "Add new book (staff)."

**Response:** Collect and validate title, author, book type, price and stock;
generate a book ID; create the `Book`; call the repository's `add`; then show a
success message. Validate type against `BOOK_TYPES`, price as positive, and stock
as a non-negative integer.

### 35. Reusable input validation

**Prompts:** "Faz uma função input_number com exceção amigável para números e uma
para float" and "Faz um input_text para validar non-empty ou mínimo 3 chars."

**Response:** Proposed small loop-based helpers that parse input, validate minimum
and maximum bounds, and print friendly `ValueError` messages. `input_text` should
strip surrounding whitespace and validate `min_length` before returning.

### 36. Book inventory management

**Prompt:** "Recapitula as opções de book_inventory_management."

**Response:** Recommended a submenu containing list books, add book, search book,
update price, update stock quantity, remove book, view low-stock books, and back.

### 37. Search and update books

**Prompts:** "Como seria o search_book?", "Update book price" and "Update stock
quantity."

**Response:** Search should normalise the term and match ID, title or author, with
an optional exact type filter. Updates should find by ID, validate the new numeric
value, modify the domain object through its public method/property, persist using
`repository.update(book)`, and report not-found errors clearly.

### 38. Remove book and low stock

**Prompts:** "Remove book", "Como fica o remove no book repo?" and "View low
stock books."

**Response:** `remove(book_id)` should filter the rows, detect whether anything
was removed, raise when the ID is absent, and rewrite the file. The UI should ask
for confirmation before removal. Low-stock reporting should use a configurable
threshold and sort by stock ascending.

### 39. User management

**Prompts:** "Recapitula o menu user management", "Register user", "Versão
customer apenas: register_new_customer", "Search user" and "Update user."

**Response:** Recommended list/search/register/update and back. Customer
registration should collect required customer fields, validate a unique email,
generate an ID, set the customer role, hash the password, and persist it. Search
could match ID, name or email. Updates should preserve immutable identifiers and
avoid overwriting fields when the user leaves an optional input blank.

### 40. Order management

**Prompts:** "Order management", "Search order by ID", "View orders by customer",
"Update order status" and "Como ficaria cancel_order?"

**Response:** Recommended list all, search by ID, filter by customer, update
status, cancel, and back. Status changes should be limited to known values.
Cancellation should reject already completed/cancelled orders and, if the chosen
business rule requires it, restore stock once before saving the cancelled status.

### 41. Reports

**Prompts:** "Reports é opção ou submenu?", "Seleciona só as melhores opções" and
"Me mostra como cada um deles ficaria."

**Response:** Recommended a reports submenu with only three high-value reports:
sales summary, low-stock books, and best-selling books. Sales summary aggregates
non-cancelled order count and revenue; low stock filters by threshold; best
sellers aggregate quantities from order items and sort descending.

### 42. Review, PEP 8, testing and lecturer login note

**Prompt:** Requested review of `main.py`, PEP 8 compliance, application testing,
an AI-use comment, and visible preconfigured lecturer test accounts. Initially
requested no direct code changes, then explicitly authorised PEP 8 changes.

**Response:** Inspected the project and assessment, identified issues and suggested
corrections, formatted the Python source to PEP 8 conventions, added a concise AI
usage disclosure/comment, exposed demonstration login records in the login UI,
and tested the main authentication paths.

### 43. Load test data feature

**Prompt:** "Adiciona no login uma opção 'load test data'. Nela cria mais dois
usuários e popula os CSVs de books e orders, com dados suficientes para testar as
funções principais."

**Response:** Added a login-menu option and an idempotent test-data loader. It
creates two additional demonstration users and enough books/orders to exercise
searching, filtering, cart/checkout, order history, status changes, reports, and
low-stock views. Verified that rerunning it does not duplicate the records and
that a generated customer can log in and view their orders.

### 44. Publishing this record

**Prompt:** "Consegue subir no repositório `til` do meu GitHub um record dos
prompts e respostas? Não sei se consegue gerar completo, mas tenta abranger o
máximo."

**Response:** Created this broad, condensed record for publication in the user's
`til` repository, excluding unrelated personal content and internal platform
messages.

## Summary of AI contribution

AI was used as a collaborative programming assistant for requirements
interpretation, menu and class design, repository architecture, Python syntax,
CSV modelling, input validation, implementation examples, code formatting,
testing guidance, test-data generation, and documentation. The student remains
responsible for reviewing, understanding, adapting, and validating all submitted
work.

