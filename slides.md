class: center, middle

# RESTful API

???

* pół na pół: sztuka i nauka
* wyklarowały się dobre praktyki
* głównie na bazie HTTP (ale nie jest to konieczność)

---

## REpresentational State Transfer

* Styl architektoniczny oparty na 6 zasadach
* Roy Fielding – [dysertacja doktorska](http://www.ics.uci.edu/~fielding/pubs/dissertation/rest_arch_style.htm)
* Oparty o zasoby
    * zasaoby a nie akcje (jak w SOAP)
    * rzeczowniki zamiast czasowników
    * /getUserData?userId=123 -> /user/123/data
    * zasób odseparowany od reprezentacji (np. DB -> XML)
* Reprezentacja
    * stan zasobu
    * zazwyczaj JSON lub XML
    * możliwość manipulacji

---

### Klient – Serwer

![](http://www.ics.uci.edu/~fielding/pubs/dissertation/client_server_style.gif)

* Zakłada rozłączenia
* Separacja zagadnień
* Jednolity interfejs łączy klienta z serwerem

---

### Bezstanowość

![](http://www.ics.uci.edu/~fielding/pubs/dissertation/stateless_cs.gif)

* Serwer nie zawiera stanu klienta
* Każda wiadomość jest w pełni samowystarczalna (może zawierać informację o stanie)
* Jakikolwiek stan jest jedynie po stronie klienta

---

### Keszowalność

![](http://www.ics.uci.edu/~fielding/pubs/dissertation/ccss_style.gif)

* Implicite
* Explicite
* Negocjowalna

---

### Jednolity interfejs

![](http://www.ics.uci.edu/~fielding/pubs/dissertation/uniform_ccss.gif)

* Kontrakt między serwerem a klientem
* Upraszcza architekturę i zmiejsza zależności
    * możliwość niezależnego rozwoju
* Oparty o zasoby

---

### System warstwowy

![](http://www.ics.uci.edu/~fielding/pubs/dissertation/layered_uccss.gif)

* Klient nie wie do jakiego serwera się łączy (końcowy czy pośredni)
* Mogą istnieć warstwy pośrednie (np. bezpieczeństwa)
* Zwiększa niezależność i skalowalność

---

### Kod na życzenie

![](http://www.ics.uci.edu/~fielding/pubs/dissertation/rest_style.gif)

* Opcjonalne
* Serwer może czasowo rozszerzyć klienta
* Transfer logiki do klienta, którą następnie wykonuje
    * np. aplety, js

---

## Metody HTTP

* Odpowiadają za dużą część "jednolitego interfejsu"
* Wprowadzenie akcji na rzeczownikowych zasobach
* Najczęstsze: POST, GET, PUT, PATCH, DELETE
* Rzadziej wykorzystywane: OPTIONS, HEAD

---

### Mapowanie metod HTTP

Metoda     | CRUD           | /customers                                | /customers/{id}
-----------|----------------|-------------------------------------------|--------------------------------------------
**POST**   | Create         | 201 (Created), Location: /customers/{id}  | 404 (Not Found), 409 (Conflict)
**GET**    | Read           | 200 (OK), lista klientów                  | 200 (OK), pojedynczy klient, 404 (Not Found)
**PUT**    | Update/Replace | 404 (Not Found), chyba, że wszystko       | 200 (OK), 204 (No Content), 404 (Not Found)
**PATCH**  | Update/Modify  | 404 (Not Found), chyba, że wszystko       | 200 (OK), 204 (No Content), 404 (Not Found)
**DELETE** | Delete         | 404 (Not Found), chyba, że wszystko       | 200 (OK), 204 (No Content), 404 (Not Found)

---

### Właściwości metod HTTP

Metoda     | Bezpieczne | Idempotentne
-----------|:----------:|:------------:
**POST**   |     -      |       -
**GET**    |     +      |       +
**PUT**    |     -      |       +
**PATCH**  |     -      |       -
**DELETE** |     -      |       +

---

## Nazewnictwo zasobów

* Dobre – łatwe i intuicyjne w używaniu
* Złe – trudne do zrozumienia i używania
* Lista zasobów
    * użytkownicy systemu
    * kursy, na które student jest zapisany
    * kolekcja postów użytkownika
    * artykuł o innowacji roku
 * Każdy zasób jest jednostką usługową, która ma co najmniej jedno URI
 * URI powinno sensownie i jednoznacznie opisywać zasób
 * URI może być hierarchiczne (z zachowaną spójnością i relacjami)
 * API jest pisane dla konsumentów, nie dla posiadanych danych
---

### Przykładowe URI

* POST http://www.example.com/customers
--

* GET http://www.example.com/customers/33245
--

* POST http://www.example.com/customers/33245/orders
--

* GET http://www.example.com/customers/33245/orders
--

* POST http://www.example.com/customers/33245/orders/8769/lineitems
--

* POST http://www.example.com/orders/8769/lineitems
--

* GET http://www.example.com/orders/8769
--

* GET http://www.example.com/customers/33245/orders/8769/lineitems/1

---

### Antywzorce nazywania zasobów

* GET http://api.example.com/services?op=update_customer&id=12345&format=json
--

* GET http://api.example.com/update_customer/12345
--

* GET http://api.example.com/customers/12345/update
--

* PUT http://api.example.com/customers/12345/update

---

## Kody statusów HTTP

* **1xx** – informacyjne
    * 101 Switching Protocols
--

* **2xx** – sukces
    * 202 Accepted
--

* **3xx** – przekierowanie
    * 301 Moved Permanently
    * 303 See Other
--

* **4xx** – błąd klienta
    * 401 Unauthorized
    * 403 Forbidden
--

* **5xx** – błąd serwera
    * 502 Bad Gateway
    * 509 Bandwidth Limit Exceeded

---

## Podpowiedzi

* Używaj method HTTP do wykonywania operacji na zasobach
    * GET, PUT / PATCH, DELETE, *POST*
--

* Dostarcz sensowne nazewnictwo zasobów
    * 80% sztuki i 20% nauki
    * /users/12345 zamiast /api?type=user&id=12345
    * używaj hierachii do określania kontekstu
    * projektuj dla klientów a nie dla danych
    * używaj rzeczowników, unikaj czasowników
    * liczba mnoga jest rekomendowana
--

* Używaj kodów statusów HTTP do określania wyniku
    * 200 OK, 201 CREATED, 202 ACCEPTED, 204 NO CONTENT
    * 400 BAD REQUEST, 401 UNAUTHORIZED, 403 FORBIDDEN, 404 NOT FOUND
    * 405 METHOD NOT ALLOWED, 409 CONFLICT
    * 500 INTERNAL SERVER ERROR


---

## Dojrzałość API RESTowego

![](http://i.stack.imgur.com/FoKRB.png)

---

### Hypermedia As The Engine Of Application State

* Dodatkowe metadane pozwalające klientowi na interakcję z serwerem
    * bez znajomości dokładnego API
    * łatwiejszy niezależny rozwój klienta i serwera

```http
GET /account/12345 HTTP/1.1
Host: somebank.org
Accept: application/xml
...
```
--

.col[
```http
HTTP/1.1 200 OK
Content-Type: application/xml
Content-Length: ...

<?xml version="1.0"?>
<account>
   <account_number>12345</account_number>
   <balance currency="usd">100.00</balance>
   <link rel="deposit" href="http://somebank.org/account/12345/deposit"/>
   <link rel="withdraw" href="http://somebank.org/account/12345/withdraw"/>
   <link rel="transfer" href="http://somebank.org/account/12345/transfer"/>
   <link rel="close" href="http://somebank.org/account/12345/close"/>
 </account>
```
]

--

.col[
```http
HTTP/1.1 200 OK
Content-Type: application/xml
Content-Length: ...

<?xml version="1.0"?>
<account>
    <account_number>12345</account_number>
    <balance currency="usd">-25.00</balance>
    <link rel="deposit" href="http://somebank.org/account/12345/deposit"/>
</account>
```
]

---

### HATEOAS i JSON

* Brak jednego standardu jak w XML-u
* Kilka konkurencyjnych
    * [**HAL+JSON**](http://stateless.co/hal_specification.html)
    * [Siren](https://github.com/kevinswiber/siren)
    * [Collection+JSON](http://amundsen.com/media-types/collection)
    * [JSON-LD](http://json-ld.org/)

--

```json
{
    "account": {
        "account_number": 12345,
        "balance":{
            "value": 100.00,
            "currency": "usd"
        }
   },
   "_links": {
       "self": { "href": "/account/12345" },
       "deposit": { "href": "/account/12345/deposit" },
       "withdraw": { "href": "/account/12345/withdraw" },
       "transfer": { "href": "/account/12345/transfer" },
       "close": { "href": "/account/12345/close" }
   }
}
```

---

class: center, middle

# Pytania?
