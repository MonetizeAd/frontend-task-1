Mini sistem koji dozvoljava login register i listanje produkata

# Header

Potrebno je u hederu svih stranica prikazati button za logout, user id i username ukoliko je logovan user, a ukoliko nije prikazati button za login in
User id i username se dobijaju iz bodia JWT-a
Ne postoji api za logout, potrebo je uraditi logout na frontendu
U hederu treba da postoji navigacija ka pocetnoj stranici i stranici za listanje produkata ( ukoliko je user logovan )

# Stranice za registraciju i logovanje

## Registracija

Nakon registracije automatski logovati usera te ga prebaciti na pocetnu stranicu
Zabraniti pristup stranici za registraciju ukoliko je user logovan

subscribeToNewsLetter treba da bude checkbox
gender treba da bude radio
status treba da bude dropdown

## Login

Nakon logina prebaciti usera na pocetnu stranicu
Zabraniti pristup stranici za login ukoliko je user logovan

# Ostale stranice

## Pocetna stranica

Stranica sluzi samo zbog navigacije, nema nikakvu funkcionalnost

## Prikaz produkata

Treba redirektat usera na login ukoliko navigira na ovu stranicu a nije logovan
Potrebno napraviti tabelu za listanje produkata
Treba postojati opcija za filtriranje produkta po nazivu i cijeni ( odvojena dva input polja )
Tabela treba imati paginaciju gdje mozemo birati koliko proizvoda zelimo da prikazemo po stranici

Produkti se dobijaju u sljedecm formatu

```json
{
    "products": {
        "1": {
            "name": "Product name1",
            "price": 101,
            "linkedProducts": {
                "2": {
                    "name": "Product name2",
                    "price": 102
                },
                "3": {
                    "name": "Product name3",
                    "price": 103
                }
            }
        },
        "2": {
            "name": "Product name2",
            "price": 102
        },
    }
}
```

Posto radimo paginaciju i filtriranje na frontendu, server neće pripremiti podatke za ispis. Treba se napraviti mehanizam pripremanja podataka za ispis u tabelu.
Treba uzeti u obzir i atribut "linkedProducts" jer se tu možda nalaze produkti koji nemaju na rootu responsa sa servera pa ih treba uvrstit, te takođe filtrirati dublikate.

# Api

Link za postman: https://www.postman.com/crimson-star-189557/workspace/monetizead-junior-task/collection/11520066-278e5f78-40d5-4841-8e7e-5988f0cf0e77?action=share&creator=11520066

Api je dostupan na https://monetizead-junior-task.herokuapp.com

## Register

### Request

`POST /api/register`

```json
{
    "username": "username",
    "password": "password",
    "repeatPassword": "password",
    "subscribeToNewsLetter": true,
    "gender": "male",
    "status": "active",
    "yearOfBirth": 20,
}
```

validacije

- username: required, min: 3, max: 20, unique
- password: required, min: 6, max: 20
- repeatPassword: required, min: 6, max: 20, same: password
- subscribeToNewsLetter: required, enum (one of): true, false
- gender: required. enum (one of): male, female, other
- status: required, enum (one of): active, inactive
- yearOfBirth: required, min: 18, max: 99

### Response

#### 200

User kreiran, nema reponse bodia

#### 400

```json
{
    "message": "Username already exists"
}
```

#### 500

Server greska, nema reponse bodia

## Login

`POST /api/login`

```json
{
    "username": "username",
    "password": "password"
}
```

validacije

- username: required, min: 3, max: 20
- password: required, min: 6, max: 20

### Response

#### 200

```json
{
    "token": "JWT"
}
```

#### 401

User ne postoji, ili su netačni podaci

#### 500

Server greska, nema reponse bodia

## Listanje produkata

`GET /api/products`
`Headers:`
    `Authorization: Bearer JWT`

### Response

#### 200

```json
{
    "products": {
        "1": {
            "name": "Product name1",
            "price": 101,
            "linkedProducts": {
                "2": {
                    "name": "Product name2",
                    "price": 102
                },
                "3": {
                    "name": "Product name3",
                    "price": 103
                }
            }
        },
        "2": {
            "name": "Product name2",
            "price": 102
        },
    }
}
```

#### 500

Server greska, nema reponse bodia


# Minimalni zahtjevi
Nije dozvoljeno koristenje component biblioteka kao tipa MUI

Dozvoljeno koristenje CSS utilitia kao sto je TaliwindCSS
Nema restrikcija ili specijalnih zahtjeva dizajna, preglednost i funkcionalnost je fokus
Obavezno koristenje funckionalnih komponenti