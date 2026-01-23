# HTML DOM — teoria i praktyka (WERSJA ROZSZERZONA, PODSTAWY)


## Co to w ogóle jest HTML DOM?

Na samym początku warto zrozumieć, że plik HTML sam w sobie jest jedynie statycznym plikiem tekstowym. Taki plik opisuje strukturę strony, jej elementy oraz ich wzajemne położenie, ale nie posiada żadnej logiki ani zdolności reagowania na działania użytkownika. Strona oparta wyłącznie na HTML nie potrafi obsłużyć kliknięcia, zmienić treści po załadowaniu ani w żaden sposób „zareagować” na to, co robi użytkownik.

Dopiero przeglądarka internetowa nadaje temu plikowi „życie”. W momencie wczytania strony przeglądarka analizuje (parsuje) kod HTML, a następnie tworzy w pamięci komputera specjalną strukturę danych. Ta struktura jest obiektową reprezentacją dokumentu HTML i nosi nazwę DOM (Document Object Model). Od tej chwili strona przestaje być tylko tekstem, a staje się zbiorem obiektów, którymi można sterować za pomocą JavaScript.

Innymi słowy: DOM to sposób, w jaki przeglądarka widzi stronę.

### Problem, który DOM rozwiązuje

Wyobraź sobie stronę HTML jako **kartkę papieru**.

HTML sam z siebie:

* nie reaguje na kliknięcia
* nie zmienia się po załadowaniu
* nie „myśli”

Gdybyśmy mieli **sam HTML**, strona byłaby martwa.

### Co robi przeglądarka?

Gdy przeglądarka:

1. wczyta plik `.html`
2. **parsuje** go
3. **tworzy strukturę w pamięci**

Ta struktura to właśnie **DOM**.

 **DOM = obiektowy model dokumentu**


### Definicja akademicka (do egzaminu)

> **HTML DOM (Document Object Model)** to hierarchiczna, drzewiasta reprezentacja dokumentu HTML w postaci obiektów, umożliwiająca jego dynamiczną manipulację za pomocą JavaScript.


## HTML ≠ DOM (bardzo ważne)

HTML i DOM bardzo często są mylone, mimo że pełnią zupełnie różne role. HTML to plik zapisany na dysku, który opisuje strukturę strony. Jest on statyczny i nie zmienia się samodzielnie w trakcie działania aplikacji. DOM natomiast powstaje dopiero po załadowaniu strony i istnieje wyłącznie w pamięci RAM przeglądarki.

JavaScript nie modyfikuje pliku HTML, ponieważ przeglądarka nie edytuje źródłowego pliku strony. Zamiast tego JavaScript manipuluje strukturą DOM, czyli obiektowym modelem dokumentu. Z tego powodu po odświeżeniu strony wszystkie zmiany wykonane przez JavaScript znikają — DOM jest tworzony od nowa na podstawie oryginalnego HTML.

Można to porównać do planu budynku i samego budynku. HTML jest planem, a DOM jest faktycznie stojącą konstrukcją, po której możemy się poruszać i którą możemy przebudowywać.

### HTML – co to jest?

HTML to:

* plik tekstowy
* zapisany na dysku
* statyczny

Przykład:

```html
<p>Hello</p>
```

To **tylko tekst**, nic więcej.

### DOM – co to jest?

DOM to:

* struktura **obiektów w pamięci RAM**
* tworzona **po załadowaniu strony**
* sterowana przez JavaScript

**JavaScript NIE ZMIENIA HTML** ==> JavaScript **zmienia DOM**

### Porównanie

| HTML             | DOM           |
| ---------------- | ------------- |
| Przepis na pizzę | Gotowa pizza  |
| Plan budynku     | Budynek       |
| Plik `.java`     | Obiekty w JVM |

Można zatem powiedzieć, że HTML to nasz pomysł, szkielet na stronę, a DOM to już istniejąca strona.

## Drzewo DOM (STRUKTURA STRONY)

DOM ma strukturę drzewa, ponieważ każdy dokument HTML składa się z elementów zagnieżdżonych jeden w drugim. Na samej górze drzewa znajduje się cały dokument, a pod nim kolejne elementy strony. Relacje pomiędzy elementami są relacjami typu rodzic–dziecko, dokładnie tak jak w strukturze drzewa.

Każdy znacznik HTML staje się w DOM osobnym węzłem. Jeżeli wewnątrz jednego znacznika znajdują się kolejne znaczniki, to w DOM będą one jego dziećmi. Taka struktura pozwala bardzo precyzyjnie określać zależności pomiędzy elementami strony oraz łatwo się po niej poruszać.

Dodatkowo w DOM występują różne typy węzłów. Najczęściej spotykane są węzły elementów (czyli znaczniki HTML), węzły tekstowe (czyli tekst znajdujący się wewnątrz znaczników) oraz węzły atrybutów. Wszystkie te elementy są traktowane jako obiekty, które można odczytywać i modyfikować.

### Dlaczego „drzewo”?

Każdy dokument HTML ma:

* jeden korzeń - znacznik HTML
* elementy zagnieżdżone - wewnątrz znacznika
* relacje rodzic–dziecko => znacznik nadrzędny i znacznik podrzędny

HTML:

```html
<html>
  <body>
    <div>
      <p>Tekst</p>
      <p>Tekst2</p>
    </div>
  </body>
</html>
```

DOM:

```
Document
 └── html
     └── body
         └── div
             └── p
             |   └── "Tekst"
             └── p
                 └── "Tekst2"
```

Jak widać łatwo przerobić kod HTML w drzewiastą strukturę znaczników DOM, która doskonale pokazuje zależności i relacje. Dla przykładu tutaj pierwszy znacznik `p` jest pierwszym dzieckiem rodzica znacznika `div`. Zarówno obydwa znaczniki `p` są zagnieżdżone w znaczniku `div`. 

**Każdy element to węzeł (node)**

### Rodzaje węzłów

1. **Element node**

   ```html
   <p>, <div>, <button>
   ```

2. **Text node**

   ```
   "Tekst w środku"
   ```

3. **Attribute node**

   ```html
   class="box"
   ```

   * ale może o tym trochę później...

> „WSZYSTKO w HTML to obiekt w DOM”


## Obiekt `document` – punkt wejścia

Aby w ogóle rozpocząć pracę z DOM, JavaScript udostępnia globalny obiekt o nazwie document. Reprezentuje on cały dokument HTML i stanowi główne wejście do drzewa DOM. Każda operacja związana z wyszukiwaniem elementów, modyfikowaniem treści czy obsługą zdarzeń rozpoczyna się właśnie od tego obiektu.

Bez użycia obiektu document JavaScript nie jest w stanie odnaleźć żadnego elementu strony. To on pozwala pobrać konkretne znaczniki, zmienić ich treść, styl lub przypisać im reakcje na działania użytkownika. Można powiedzieć, że document jest „uchwytem” do całej strony.

### Czym jest `document`?

`document` to:

* główny obiekt DOM
* reprezentuje **cały dokument HTML**
* dostępny globalnie w JS

Bez `document`:
* nie znajdziesz elementu
* nie zmienisz tekstu
* nie obsłużysz kliknięcia

> Będzie to przydatne później w JS'ie ;)

### Przykłady w JS

```js
document.title = "Nowy tytuł";
document.body.style.backgroundColor = "lightgray";
```

Strona reaguje natychmiast po JS modyfikuje DOM, nie HTML.

W powyższym przykładzie ustawiamy tytuł i kolor tła w stylach ciała strony (czyli znacznika `<body>`).

## Wyszukiwanie elementów w DOM

JavaScript nie ma domyślnej wiedzy o tym, który element strony chcemy zmodyfikować. Zanim możliwa jest jakakolwiek zmiana, należy odnaleźć konkretny element w strukturze DOM. Do tego celu wykorzystuje się identyfikatory, klasy oraz znaczniki HTML.

Identyfikator (id) służy do jednoznacznego oznaczenia pojedynczego elementu. W całym dokumencie nie powinny istnieć dwa elementy o tym samym id. Klasy (class) pełnią inną rolę — służą do grupowania elementów. Wiele elementów może posiadać tę samą klasę, a pojedynczy element może mieć przypisanych kilka klas jednocześnie.

Po nadaniu elementom identyfikatorów lub klas JavaScript może je wyszukiwać przy pomocy odpowiednich metod. Najprostszą z nich jest getElementById, która zwraca dokładnie jeden element. Bardziej nowoczesnym i uniwersalnym rozwiązaniem jest querySelector, który korzysta z selektorów CSS i pozwala na bardzo precyzyjne wyszukiwanie elementów. Wariant querySelectorAll umożliwia pobranie wielu elementów naraz, np. wszystkich elementów należących do danej klasy.

### Dlaczego musimy „szukać”?

JS **nie wie**, który element chcesz zmienić.

Musisz mu powiedzieć:

* po ID
* po klasie
* po znaczniku

... tyle, że domyślnie znaczniki te nie posiadają ID, ani klasy, chyba, że nadamy im to ręcznie:

#### ID

```html
<p id="tutajDefiniujeszID"></p>
```

#### Klasa

```html
<p class="tutajDefiniujeszKlasę"></p>
```

#### Porównanie

* ID powinno być unikalne dla każdego obiektu DOM.
    ```html
    <p id="paragraf"></p>
    <p id="paragraf"></p> <!-- Błąd, po to samo id co paragraf wyżej -->
    <p id="paragraf2"></p>
    ```
* klasy to grupy obiektów, więc wiele obiektów DOM może mieć tą samą klasę. Mało tego jedne obiekt DOM może posiadać wiele klas jednocześnie.
    ```html
    <p class="ozdobny"></p>
    <p class="ozdobny"></p> <!-- Tu już można ;) -->
    <button class="ozdobny wielki"></button> <!-- Tak też można bo mam dwie klasy "ozodbny" i "wielki" ;) -->
    ```
* obiekt DOM może posiadać zarówno ID jak i klasę
    ```html
    <p class="ozdobny" id="paragraf"></p>
    ```

No to teraz jak mamy już ID i klasy to możemy się nauczyć, jak pobierać obiekty DOM po ID, klasie, itd. a następnie je modyfikować.

### `getElementById` – NAJPROSTSZE, ale trochę przestarzałe

HTML:

```html
<p id="info">Tekst</p>
```

JS:

```js
const p = document.getElementById("info");
```

Zasady:

* ID musi być **unikalne**
* zawsze zwraca **JEDEN element**

Podobnie działa `getElementByClass`, ale nie zwraca tylko jednego elementu, co może  być problematyczne (albo czasem przydatne).

### 5.3 `querySelector` – NAJLEPSZE, ale skomplikowane

Działa trochę jak kwerendy bazodanowe - musimy napisać odpowiednią regułę, aby wybrać nasz dane. 

TYLE, ŻE....

W `getElementById` używamy samego ID, a w `querySelector` używamy selektorów z `CSS` (więc więcej rozjaśni się później ;) ).

```js
document.querySelector("p");
document.querySelector(".box");
document.querySelector("#info");
```

### `querySelectorAll`

```js
const items = document.querySelectorAll(".item");
```

Zwraca listę elementów

Można robić:

```js
items.forEach(el => {
  el.style.color = "red";
});
```

Czyli dla każdego elementu wybranego z `querySelectorAll(".item")` zmieniamy jego kolor w stylach na _red_.

## Modyfikowanie elementów

Po odnalezieniu elementu w DOM możliwe jest modyfikowanie jego zawartości. Najprostszym sposobem jest zmiana tekstu przy pomocy właściwości textContent. Powoduje ona całkowite zastąpienie poprzedniej treści nowym tekstem, bez interpretowania znaczników HTML.

Alternatywą jest innerHTML, które pozwala wstawić do elementu kod HTML. Należy jednak pamiętać, że metoda ta wstrzykuje kod bezpośrednio do DOM, co w nieodpowiednich warunkach może prowadzić do problemów z bezpieczeństwem. Z tego powodu innerHTML należy stosować świadomie i ostrożnie.

### Zmiana tekstu

```js
element.textContent = "Nowy tekst";
```

* Usuwa starego teksta
* Wstawia nowego

Bez HTML, bez znaczników, czysta zawartość.

### `innerHTML`

A tutaj już wstrzykujemy zawartość HTML w zawartość

```js
element.innerHTML = "<strong>Tekst</strong>";
```

## Style i klasy CSS

Choć JavaScript umożliwia bezpośrednią zmianę stylów elementów, nie jest to zalecane rozwiązanie. Nadawanie stylów bezpośrednio w JS prowadzi do nieczytelnego i trudnego w utrzymaniu kodu. Lepszym podejściem jest definiowanie stylów w CSS, a następnie sterowanie nimi poprzez dodawanie i usuwanie klas CSS w JavaScript.

Takie podejście rozdziela odpowiedzialności: JavaScript odpowiada za logikę i zachowanie strony, a CSS za jej wygląd. Dzięki temu kod jest bardziej przejrzysty i łatwiejszy do rozbudowy.

### Zmiana stylu bezpośrednio

Jeśli mamy wybrany element to możemy nadawać mu style....

```js
element.style.color = "red";
element.style.fontSize = "20px";
```

...Tyle, że jest to:
* brzydkie
* nieczytelne
* trudne w utrzymaniu

To jak powinnismy to robić?

### 7.2 Praca na klasach (POPRAWNIE)

Najpierwej tworzymy klasy w CSS.

```css
.active {
  color: red;
}
```
A nastęnie w skrypcie JS'a dodajemy do elementu klasę, dzięki której przybierze nasze wymarzone właściwiości/ style.

```js
element.classList.add("active");
element.classList.remove("active");
element.classList.toggle("active");
```

> **JS steruje, CSS wygląda**

---

## Zdarzenia (EVENTY)

Strona internetowa staje się interaktywna dopiero wtedy, gdy potrafi reagować na działania użytkownika. Taką reakcję nazywamy zdarzeniem. Zdarzeniem może być kliknięcie przycisku, wpisanie tekstu, najechanie kursorem czy wysłanie formularza.

JavaScript umożliwia nasłuchiwanie takich zdarzeń za pomocą metody addEventListener. Mechanizm ten polega na przypisaniu do konkretnego elementu funkcji, która zostanie wykonana w momencie wystąpienia danego zdarzenia. Dzięki temu możliwe jest dynamiczne sterowanie zachowaniem strony.

### Co to jest zdarzenie?

Zdarzenie = reakcja na użytkownika:

* kliknięcie
* wpisanie tekstu
* najechanie myszką

Bez eventów strona jest martwa -> statyczna, nudna.

### `addEventListener`

```js
button.addEventListener("click", function () {
  alert("Klik!");
});
```

Co tu się dzieje?

1. do elementu, który wcześniej pewnie wyslektowaliśmy do selektora button dodajemy event listener, który
2. nasłuchuje aż user kliknie ten przycisk (`click`)
3. i gdy kliknie to wykona funkcję w tym wypadku zdefiniowaną _in-place_, która otwiera na stronie `alert` z komunikatem `Klik!`

## Tworzenie elementów w locie

DOM pozwala nie tylko modyfikować istniejące elementy, ale również tworzyć zupełnie nowe w trakcie działania strony. JavaScript może utworzyć nowy element, ustawić jego właściwości i dołączyć go do istniejącej struktury DOM. Taki element nie musi być wcześniej zapisany w HTML.

Ten mechanizm stanowi podstawę działania dynamicznych aplikacji webowych, takich jak listy zadań, systemy komentarzy czy karty produktów ładowane bez przeładowania strony.

Czysto teoretycznie używając DOM i JS'a możemy dodawać elementy "w locie", tzn. w trakcie działania już strony dodać nowy element poprzez zdefiniowanie go jako nowego elementu obiektu DOM.

```js
const p = document.createElement("p");
p.textContent = "Nowy paragraf";

document.body.appendChild(p); //Tutaj dodajemy ten nowey element jako nazstępne dziecko znacnzika <body>
```

HTML **nie musiał istnieć wcześniej**

To podstawa dla stron obsługujących:

* tzw. TODO list
* komentarze
* karty produktów