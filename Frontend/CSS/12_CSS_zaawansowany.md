# CSS – słowa kluczowe, jednostki, selektory i właściwości (materiał pełny, egzaminacyjny)

## 1. Słowa kluczowe w CSS (`inherit`, `initial`, `unset`)

CSS posiada **słowa kluczowe**, które kontrolują sposób dziedziczenia i resetowania stylów.

### `inherit`

Słowo kluczowe `inherit` oznacza:

> **przejmij wartość tej właściwości od elementu nadrzędnego (rodzica)**.

Przykład:

```css
p {
  color: inherit;
}
```

Jeżeli rodzic ma kolor czerwony, dziecko też będzie czerwone.

---

### `initial`

`initial` ustawia **wartość domyślną przeglądarki**, ignorując styl rodzica.

```css
p {
  color: initial;
}
```

---

### `unset`

`unset`:

* jeśli właściwość jest dziedziczona → zachowuje się jak `inherit`
* jeśli nie → jak `initial`

Jest to rozwiązanie pośrednie.

---

## 2. Jednostki długości w CSS

Jednostki określają **rozmiary elementów, odstępy, czcionki**.

---

### Jednostki bezwzględne

```css
width: 100px;
```

* `px` – piksele (najczęstsze)
* `cm`, `mm`, `in` – rzadko używane

NIERESPONSYWNE

---

### Jednostki względne (WAŻNE)

```css
font-size: 1.2em;
width: 50%;
```

* `%` – procent względem rodzica
* `em` – względem czcionki rodzica
* `rem` – względem czcionki elementu `html`
* `vw`, `vh` – procent szerokości/wysokości ekranu

PODSTAWA RESPONSYWNOŚCI

---

## 3. Responsywność – co to znaczy?

Strona responsywna:

* dostosowuje się do rozmiaru ekranu,
* działa na telefonie, tablecie i komputerze,
* nie wymaga powiększania.

CSS wspiera responsywność przez:

* jednostki względne,
* media queries,
* elastyczne układy.

---

## 4. Definiowanie kolorów w CSS

### Nazwy kolorów

```css
color: red;
```

---

### HEX (najczęstsze)

```css
color: #ff0000;
```

każde dwie cyfry szesnatkowe okreslają nasycenie RGB od 0 do 255 (wyjaśnienie patrz niżej)

---

### RGB / RGBA

```css
color: rgb(255, 0, 0);
color: rgba(255, 0, 0, 0.5);
```
gdzie A to przezroczystość, R to nasycenie kolorem czerownym, G zielonym i B niebieskim - z zestawienia tych 3 kolorów możemy stworzyć każdy kolor :)

---

### HSL / HSLA

```css
color: hsl(0, 100%, 50%);
```

HSL (Hue, Saturation, Lightness – Odcień, Nasycenie, Jasność)

---

## 5. Dziedziczenie w CSS

Nie wszystkie właściwości się dziedziczą.

Dziedziczą się m.in.:

* `color`
* `font-family`
* `font-size`

Nie dziedziczą się:

* `margin`
* `padding`
* `border`

Dziedziczenie można wymusić przez `inherit`.

---

## 6. Łączenie selektorów CSS

### Selektor potomka

```css
div p { color: red; }
```

---

### Selektor dziecka

```css
div > p { color: blue; }
```

---

### Łączenie selektorów (AND)

```css
p.highlight { color: green; }
```

---

### Lista selektorów (OR)

```css
h1, h2, h3 { font-family: Arial; }
```

---

## 7. Pseudoklasy (`:hover`, `:active`, itd.)

Pseudoklasy opisują **stan elementu**.

```css
a:hover { color: red; }
a:active { color: blue; }
```

Najważniejsze:

* `:hover` – najechanie myszą
* `:active` – kliknięcie
* `:focus` – aktywne pole
* `:visited` – odwiedzony link

---

## 8. Właściwości CZCIONKI

```css
p {
  font-family: Arial;
  font-size: 16px;
  font-weight: bold;
  font-style: italic;
  line-height: 1.5;
  text-align: justify;
  text-decoration: underline;
}
```

---

## 9. Właściwości TŁA

```css
div {
  background-color: #eee;
  background-image: url("img.jpg");
  background-repeat: no-repeat;
  background-size: cover;
}
```

---

## 10. MARGINESY I ODSTĘPY

```css
div {
  margin: 10px;
  padding: 15px;
}
```

Margin – odstęp na zewnątrz
Padding – odstęp wewnętrzny

---

## 11. OBRAMOWANIA

```css
div {
  border: 2px solid black;
  border-radius: 10px;
}
```

---

## 12. ROZMIARY

```css
div {
  width: 300px;
  height: auto;
  max-width: 100%;
}
```

---

## 13. POZYCJONOWANIE

```css
.box {
  position: relative;
}
```

Rodzaje:

* static (domyślne)
* relative
* absolute
* fixed
* sticky

---

## 14. WYŚWIETLANIE ELEMENTÓW

```css
span { display: inline; }
div { display: block; }
```

Inne:

* none
* inline-block
* flex

---

## 15. LISTY

```css
ul {
  list-style-type: square;
}
```

---

## 16. TABELE

```css
table {
  border-collapse: collapse;
}
```

---

## 17. Najważniejsze na egzamin

* `inherit` = dziedziczenie
* jednostki względne = responsywność
* pseudoklasy = stan
* CSS nie zmienia treści
