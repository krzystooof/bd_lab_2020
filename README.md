# bd_lab_2020
## SQL#2 zestaw: `bd_lab_2_excersises.pdf`  
### 1. Utwórz nową bazę danych o nazwie uczelnia oraz wybierz ją (USE ...).

```
CREATE DATABASE uczelnia;
USE uczelnia;
```

### 2. Utwórz nową tabelę o nazwie studenci zawierającą następujące kolumny:
– nr_indeksu (klucz główny),           
– imie (ciąg znaków, niepuste),            
– nazwisko (ciąg znaków, niepuste),           
– adres (ciąg znaków, niepuste),            
– narodowosc (ciąg znaków, niepuste, domyślnie ‘Polska’).

```
Create Table studenci(
	nr_indeksu int not null,
	imie varchar(20) not null,
	nazwisko varchar(20) not null,
	adres varchar(40) not null,
	narodowosc varchar(20) default 'Polska',
	primary key (nr_indeksu))
```

### 3. Utwórz nową tabelę o nazwie wykladowcy zawierającą następujące kolumny:
– wykladowca_id (klucz główny),    
– imie (ciąg znaków, niepuste),    
– nazwisko (ciąg znaków, niepuste).

```
Create Table wykladowcy(
	wykladowca_id int not null,
	imie varchar(20) not null,
	nazwisko varchar(20) not null,
	primary key (wykladowca_id))
```

### 4. Utwórz nową tabelę o nazwie kierunki zawierającą następujące kolumny:
– kierunek_id (klucz główny),     
– nazwa (ciąg znaków, niepuste).

```
Create Table kierunki(
	kierunek_id int not null,
	nazwa varchar(20) not null,
	primary key (kierunek_id))
```

### 5. Utwórz nową tabelę o nazwie przedmioty zawierającą następujące kolumny:
– przedmiot_id (klucz główny),     
– kierunek_id (klucz obcy z tabeli kierunki, niepuste),    
– wykladowca_id (klucz obcy z tabeli wykladowcy, niepuste),    
– nazwa (ciąg znaków, niepuste).

```
Create Table przedmioty(
	przedmiot_id int not null,
	kierunek_id int not null FOREIGN KEY REFERENCES kierunki (kierunek_id), 
	wykladowca_id int not null FOREIGN KEY REFERENCES  wykladowcy (wykladowca_id),
	nazwa varchar(20) not null,
	primary key (przedmiot_id))
```

### 6. Utwórz nową tabelę o nazwie studenci_przedmioty zawierającą następujące kolumny:

– nr_indeksu (klucz obcy z tabeli studenci),   
– przedmiot_id (klucz obcy z tabeli przedmioty),   
– (klucz główny tej tabeli powinny stanowić nr_indeksu oraz przedmiot_id). 

```
Create Table studenci_przedmioty(
	nr_indeksu int not null FOREIGN KEY REFERENCES studenci (nr_indeksu),
	przedmiot_id int not null FOREIGN KEY REFERENCES przedmioty (przedmiot_id), 
	primary key (nr_indeksu,przedmiot_id))
```

### 7. Utwórz nową tabelę o nazwie oceny zawierającą następujące kolumny:
– ocena_id (klucz główny),   
– nr_indeksu (klucz obcy z tabeli studenci, niepuste),   
– przedmiot_id (klucz obcy z tabeli przedmioty, niepuste),   
– wartosc (liczba z częściami dziesiętnymi, niepuste, domyślnie ‘2.0’),   
– data (data, niepuste, domyślnie aktualna data) (GETDATE()).

```
Create Table oceny(
	ocena_id int not null primary key,
	nr_indeksu int not null FOREIGN KEY REFERENCES studenci (nr_indeksu),
	przedmiot_id int not null FOREIGN KEY REFERENCES przedmioty (przedmiot_id), 
	wartosc decimal not null default '2.0',
	data date not null default getdate())
```

### 8. Zmodyfikuj tabele (ALTER TABLE ...) dodając:
– (tabela oceny) ograniczenie niepozwalające na wstawianie ocen o wartościach mniejszych
niż 2.0 oraz większych niż 5.0,   

```
ALTER TABLE oceny
ADD CONSTRAINT spr_ocena CHECK  (wartosc>=2.0 and wartosc <= 5.0)
```

– (tabela przedmioty) ograniczenie niepozwalające na dodawanie przedmiotów o takich sa-
mych nazwach (UNIQUE),   

```
ALTER TABLE przedmioty
ADD CONSTRAINT unq_przedmiot Unique (nazwa)
```

– (tabela kierunki) ograniczenie niepozwalające na dodawanie kierunków o takich samych
nazwach,   

```
ALTER TABLE kierunki
ADD CONSTRAINT unq_kierunek Unique (nazwa)
```

– (tabela kierunki) nową kolumnę o nazwie opis typu text, z domyślą wartością równą
‘brak opisu’. 

```
ALTER TABLE kierunki
ADD opis text default 'brak opisu'
```


### 9. Wypełnij utworzone wcześniej tabele przykładowymi danymi (polecenie INSERT), tak aby każda tabela zawierała co najmniej trzy wiersze (krotki).

```
INSERT INTO kierunki(kierunek_id, nazwa)
values ('1', 'Informatyka')
INSERT INTO kierunki(kierunek_id, nazwa)
values ('2', 'Automatyka')
INSERT INTO kierunki(kierunek_id, nazwa)
values ('3', 'Robotyka')
```
```
INSERT INTO studenci(nr_indeksu, imie, nazwisko, adres)
values ('1', 'Jan', 'Anioł', 'ul. Słoneczna 10 Poznan')
INSERT INTO studenci(nr_indeksu, imie, nazwisko, adres)
values ('2', 'Jakub', 'Anioł', 'ul. Słoneczna 10 Poznan')
INSERT INTO studenci(nr_indeksu, imie, nazwisko, adres)
values ('3', 'Jerzy','Anioł', 'ul. Słoneczna 10 Poznan')
```
```
INSERT INTO wykladowcy(wykladowca_id, imie, nazwisko)
values ('1', 'John', 'Anioł')
INSERT INTO wykladowcy(wykladowca_id, imie, nazwisko)
values ('2', 'Jeremy', 'Anioł')
INSERT INTO wykladowcy(wykladowca_id, imie, nazwisko)
values ('3', 'JB','Anioł')
```
```
INSERT INTO przedmioty(przedmiot_id, kierunek_id, wykladowca_id, nazwa)
values ('1', '1', '1', 'Przedmiot 1')
INSERT INTO przedmioty(przedmiot_id, kierunek_id, wykladowca_id, nazwa)
values ('2', '2', '2', 'Przedmiot 2')
INSERT INTO przedmioty(przedmiot_id, kierunek_id, wykladowca_id, nazwa)
values ('3', '3', '3', 'Przedmiot 3')
```
```
INSERT INTO oceny(ocena_id, nr_indeksu, przedmiot_id, wartosc)
values ('1', '1','1','2')
INSERT INTO oceny(ocena_id, nr_indeksu, przedmiot_id, wartosc)
values ('2', '2','2','3')
INSERT INTO oceny(ocena_id, nr_indeksu, przedmiot_id, wartosc)
values ('3', '3','3','4')
```

### 10. Zmień wszystkie oceny (wszystkim studentom) o wartości 2.0 na 3.0 (UPDATE).

```
UPDATE oceny
SET wartosc = 3.0
WHERE wartosc = 2.0; 
```

## SQL#3 zestaw: `bd_lab_3_exercises.pdf`  

### 1. Utwórz nowy widok o nazwie ‘Products From Japan’ zawierający produkty dostarczane przez dostawców z Japonii oraz wypisz jego zawartość. 

### 2. Wyświetl produkty, których nie zamówił żaden klient (użyj NOT IN). 
### 3. Dodaj nowego klienta ‘Firma z Polski’, a następnie wyświetl nazwy ﬁrm klientów, którzy nie złożyli żadnego zamówienia (użyj OUTER JOIN). 
### 4. Wyświetl wszystkie możliwe pary (iloczyn kartezjański) imion i nazwisk pracowników (FirstName, LastName). 
### 5. Wyświetl łączną ilość zamówień każdego produktu.
### 6. Wyświetl nazwy ﬁrm klientów (CompanyName) oraz kategorie zamówionych przez nie produktów (CategoryName). 
### 7. Wyświetl nazwy ﬁrm klientów (CompanyName) oraz ilość wszystkich zamówionych produktów, posortuj wyniki malejąco. 
### 8.  Zapisz do nowej tabeli o nazwie ‘Purchases’ (nie używaj CREATE TABLE ...) informacje o tym kto zamówił produkt (CompanyName i ContactName), nazwę zamówionego produktu (ProductName) oraz imię i nazwisko pracownika związanego z danym zamówieniem.
### 9.  Skopiuj informacje o dostawcach (Suppliers), którzy dostarczają produkty kategorii o nazwie ‘Seafood’ do tabeli spedytorów (Shippers).
### 10.  Utwórz nowy widok o nazwie ‘Orders Counts’ zawierający identyﬁkator zamówienia oraz ilość produktów wchodzących w jego skład.
### 11. * Zwiększ o 5% ceny produktom, które zostały zakupione przez co najmniej pięciu różnych klientów.
