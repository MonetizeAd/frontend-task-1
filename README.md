###### Mini sistem koji dozvoljava login register i listanje produkata

# Header

- Ukoliko je user logovan u headeru treba da se ispise njegovo ime i id, te button za logout
- Ukoliko user nije logovan treba da se ispise button za login
- User id i username se dobijaju iz body-a JWT-a
- Ne postoji api za logout, potrebo je uraditi logout na frontendu
- U headeru treba da postoji navigacija ka pocetnoj stranici ( logovanim i ne logovanim userima ) i stranici za listanje produkata ( logovanim userima )

# Stranice za registraciju i logovanje

## Registracija

- Nakon registracije automatski logovati usera te ga prebaciti na pocetnu stranicu
- Zabraniti pristup stranici za registraciju ukoliko je user logovan

- subscribeToNewsLetter treba da bude checkbox
- gender treba da bude radio
- status treba da bude dropdown

## Login

- Nakon logina prebaciti usera na pocetnu stranicu
- Zabraniti pristup stranici za login ukoliko je user logovan

# Ostale stranice

## Pocetna stranica

- Stranica sluzi samo zbog navigacije, nema nikakvu funkcionalnost

## Prikaz produkata

- Treba redirektat usera na login ukoliko navigira na ovu stranicu a nije logovan
- Potrebno napraviti tabelu za listanje produkata
- Treba postojati opcija za filtriranje produkta po nazivu i cijeni ( odvojena dva input polja )
- Tabela treba imati paginaciju gdje mozemo birati koliko proizvoda zelimo da prikazemo po stranici

Produkti se dobijaju u sljedecem formatu

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

Posto radimo paginaciju i filtriranje na frontendu, server nece pripremiti podatke za ispis. Treba se napraviti mehanizam pripremanja podataka za ispis u tabelu. Treba uzeti u obzir i atribut "linkedProducts" jer se tu mozda nalaze produkti koji nemaju na rootu responsa sa servera pa ih treba uvrstit, te takodje filtrirati dublikate. Product id je attribute key u responsu (`name: "Product name1", id: "1"`).

# Api

Postman url: `https://www.postman.com/crimson-star-189557/workspace/monetizead-junior-task/collection/11520066-278e5f78-40d5-4841-8e7e-5988f0cf0e77?action=share&creator=11520066`

Backend api url: `https://junior-test.mntzdevs.com`

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
- yearOfBirth: required, min: 1900, max: 2024

### Response

#### 201

User kreiran, nema reponse body-a

#### 400

```json
{
    "message": "Username already exists"
}
```

#### 500

Server greska, nema reponse body-a

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
    "jwt": "JWT"
}
```

#### 401

User ne postoji, ili su netacni podaci

#### 500

Server greska, nema reponse body-a

## Listanje produkata

`GET /api/products`

### Headers

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

#### 401

Ne postoji JWT u headeru

#### 403

JWT nije validan

#### 500

Server greska, nema reponse body-a

# Zahtjevi

- Nije dozvoljeno koristenje component biblioteka kao tipa MUI

- Dozvoljeno koristenje CSS utilitia kao sto je TaliwindCSS
- Nema restrikcija ili specijalnih zahtjeva dizajna, preglednost i funkcionalnost je fokus
- Koristiti react ili nextJS

# Bonus

- Koristiti typescript
- Koristiti redux
- Koristiti react-query
- Koristiti axios

ukoliko niste upoznati sa gore navedenim bibliotekama, slobodno ih ignorisite

# PS

- Ukoliko imate bilo kakvih pitanja posaljite email ili otvorite issue na repozitoriju