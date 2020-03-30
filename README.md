# bd_lab_2020
## SQL#2
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

### 6. Utwórz nową tabelę o nazwie studenci_przedmioty zawierającą następujące ko-
lumny:

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

– (tabela przedmioty) ograniczenie niepozwalające na dodawanie przedmiotów o takich sa-
mych nazwach (UNIQUE),

– (tabela kierunki) ograniczenie niepozwalające na dodawanie kierunków o takich samych
nazwach,
– (tabela kierunki) nową kolumnę o nazwie opis typu text, z domyślą wartością równą
‘brak opisu’.

### 9. Wypełnij utworzone wcześniej tabele przykładowymi danymi (polecenie INSERT), tak aby każda tabela zawierała co najmniej trzy wiersze (krotki).

### 10. Zmień wszystkie oceny (wszystkim studentom) o wartości 2.0 na 3.0 (UPDATE).
