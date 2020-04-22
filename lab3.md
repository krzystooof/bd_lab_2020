## SQL#3 zestaw: `bd_lab_3_exercises.pdf`  

### 1. Utwórz nowy widok o nazwie ‘Products From Japan’ zawierający produkty dostarczane przez dostawców z Japonii oraz wypisz jego zawartość. 

```
CREATE View [Products From Japan] AS
Select *
from Suppliers
Where Suppliers.Country = 'Japan'
```

### 2. Wyświetl produkty, których nie zamówił żaden klient (użyj NOT IN). 

```
Select *
from Products
where Products.ProductID
NOT IN (
	Select ProductID
	from [Order Details]
	)
```

### 3. Dodaj nowego klienta ‘Firma z Polski’, a następnie wyświetl nazwy ﬁrm klientów, którzy nie złożyli żadnego zamówienia (użyj OUTER JOIN). 

```
INSERT into Customers(CustomerID, CompanyName)
values ('92', 'Firma z Polski')
Select *
from Customers
full Outer join Orders on Customers.CustomerID = Orders.CustomerID
where OrderID IS NULL
```

### 4. Wyświetl wszystkie możliwe pary (iloczyn kartezjański) imion i nazwisk pracowników (FirstName, LastName). 

```
Select Employees.LastName, Employees.FirstName
from Employees
Cross join Employees as EmployeesCROSS;
```

### 5. Wyświetl łączną ilość zamówień każdego produktu.

```
select Products.ProductName, COUNT(Products.ProductName)
from Products
join [Order Details] on Products.ProductID = [Order Details].ProductID
Group by ProductName
```

### 6. Wyświetl nazwy ﬁrm klientów (CompanyName) oraz kategorie zamówionych przez nie produktów (CategoryName). 

```
select Customers.CompanyName, Categories.CategoryName
from Customers
join Orders on Orders.CustomerID = Customers.CustomerID
join [Order Details] on Orders.OrderID = [Order Details].OrderID
join Products on Products.ProductID = [Order Details].ProductID
join Categories on Categories.CategoryID = Products.ProductID
```

### 7. Wyświetl nazwy ﬁrm klientów (CompanyName) oraz ilość wszystkich zamówionych produktów, posortuj wyniki malejąco. 

```
select Customers.CompanyName, count(Products.ProductID)
from Customers
join Orders on Orders.CustomerID = Customers.CustomerID
join [Order Details] on Orders.OrderID = [Order Details].OrderID
join Products on Products.ProductID = [Order Details].ProductID
Group by Customers.CompanyName
order by count(Products.ProductID)
```

### 8.  Zapisz do nowej tabeli o nazwie ‘Purchases’ (nie używaj CREATE TABLE ...) informacje o tym kto zamówił produkt (CompanyName i ContactName), nazwę zamówionego produktu (ProductName) oraz imię i nazwisko pracownika związanego z danym zamówieniem.

```
select Customers.CompanyName, Customers.ContactName, Products.ProductName, Employees.LastName, Employees.FirstName
Into [Purchases] from Customers, Products, Employees
```

### 9.  Skopiuj informacje o dostawcach (Suppliers), którzy dostarczają produkty kategorii o nazwie ‘Seafood’ do tabeli spedytorów (Shippers).

```
Insert  Into Shippers
Select * from Suppliers
join Products on Products.SupplierID = Suppliers.SupplierID
join Categories on Categories.CategoryID = Products.CategoryID
where Categories.CategoryName = 'Seafood'
```

### 10.  Utwórz nowy widok o nazwie ‘Orders Counts’ zawierający identyﬁkator zamówienia oraz ilość produktów wchodzących w jego skład.

```
CREATE View [Orders Counts] AS
Select [Order Details].OrderID, COUNT([Order Details].ProductID) as Amount
from [Order Details]
Group by [Order Details].OrderID
```

### 11. * Zwiększ o 5% ceny produktom, które zostały zakupione przez co najmniej pięciu różnych klientów.
