# Podstawowe znaczniki HTML

## 1. Czym są znaczniki HTML?

Znaczniki HTML (ang. *HTML tags*) to **specjalne słowa kluczowe**, zapisane w nawiasach ostrych `< >`, które:

* opisują strukturę strony
* informują przeglądarkę **co jest czym**
* nie są wyświetlane użytkownikowi (same znaczniki)

HTML **nie opisuje wyglądu**, tylko **znaczenie i strukturę treści**.

Przykład znacznika:

```html
<p>To jest paragraf</p>
```

* `<p>` – znacznik otwierający
* `</p>` – znacznik zamykający
* tekst pomiędzy nimi to **zawartość elementu**

---

## 2. Podstawowa struktura dokumentu HTML

Każdy poprawny dokument HTML ma **określoną, obowiązkową strukturę**.

```html
<!DOCTYPE html>
<html>
<head>
  <title>Tytuł strony</title>
</head>
<body>
  Treść strony
</body>
</html>
```

### Co tu się dzieje?

* `<!DOCTYPE html>`
  Informuje przeglądarkę, że dokument jest zgodny z HTML5.

* `<html>`
  Jest **korzeniem dokumentu**. Wszystkie inne znaczniki znajdują się w jego wnętrzu.

* `<head>`
  Zawiera **informacje techniczne**, a nie treść widoczną dla użytkownika.

* `<body>`
  Zawiera **całą widoczną treść strony**.

Na egzaminie:
➡ Brak `<!DOCTYPE html>` = dokument może być interpretowany niepoprawnie.

---

## 3. Znacznik `<head>` – informacje o stronie

Znacznik `<head>` służy do przechowywania **metadanych**, czyli informacji *o* stronie, a nie samej treści.

Najczęściej używane znaczniki w `<head>`:

### `<title>`

```html
<title>Moja strona</title>
```

* ustawia tytuł zakładki w przeglądarce
* jest używany przez wyszukiwarki (SEO)

---

### `<meta>`

```html
<meta charset="UTF-8">
```

* ustawia kodowanie znaków
* zapobiega problemom z polskimi znakami

---

### `<link>`

```html
<link rel="stylesheet" href="style.css">
```

* łączy plik CSS z dokumentem HTML
* **nie ma znacznika zamykającego**

---

## 4. Znacznik `<body>` – zawartość strony

W znaczniku `<body>` znajduje się **wszystko, co widzi użytkownik**.

---

## 5. Znaczniki nagłówków `<h1>` – `<h6>`

Nagłówki służą do **hierarchicznego porządkowania treści**.

```html
<h1>Tytuł główny</h1>
<h2>Podtytuł</h2>
<h3>Podrozdział</h3>
```

Znaczenie:

* `<h1>` – najważniejszy nagłówek strony
* `<h6>` – najmniej ważny

Zastosowanie:

* struktura dokumentu
* SEO
* czytelność

⚠️ **Nie używa się nagłówków do stylowania tekstu!**

---

## 6. Paragraf `<p>`

Znacznik `<p>` służy do tworzenia **akapitów tekstu**.

```html
<p>To jest przykładowy paragraf.</p>
```

Zastosowanie:

* tekst artykułów
* opisy
* treść stron

Każdy `<p>` tworzy **oddzielny blok tekstu**.

---

## 7. Znacznik `<br>` – nowa linia

```html
Linia 1<br>
Linia 2
```

* wstawia **przejście do nowej linii**
* nie posiada znacznika zamykającego

Na egzaminie:
➡ `<br>` nie służy do robienia odstępów wizualnych (od tego jest CSS)

---

## 8. Znacznik `<hr>` – linia pozioma

```html
<hr>
```

* wstawia poziomą linię
* oznacza **tematyczne oddzielenie treści**

---

## 9. Listy HTML

### Lista nieuporządkowana `<ul>`

```html
<ul>
  <li>Element 1</li>
  <li>Element 2</li>
</ul>
```

* lista punktowana
* kolejność nie ma znaczenia

---

### Lista uporządkowana `<ol>`

```html
<ol>
  <li>Krok 1</li>
  <li>Krok 2</li>
</ol>
```

* lista numerowana
* kolejność ma znaczenie

---

### Element listy `<li>`

* występuje **tylko wewnątrz `<ul>` lub `<ol>`**
* reprezentuje pojedynczy element listy

---

## 10. Linki – znacznik `<a>`

Znacznik `<a>` służy do tworzenia **hiperłączy**.

```html
<a href="https://example.com">Kliknij mnie</a>
```

Atrybuty:

* `href` – adres docelowy
* tekst między znacznikami jest klikalny

Link do innej strony:

```html
<a href="kontakt.html">Kontakt</a>
```

---

## 11. Obrazy – znacznik `<img>`

```html
<img src="obraz.jpg" alt="Opis obrazu">
```

Znaczenie:

* `src` – ścieżka do obrazu
* `alt` – tekst alternatywny (dostępność, SEO)

⚠️ `<img>` nie ma znacznika zamykającego

---

## 12. Kontenery: `<div>` i `<span>`

### `<div>` – kontener blokowy

```html
<div>
  <p>Tekst</p>
</div>
```

* nie ma znaczenia semantycznego
* służy do grupowania elementów
* bardzo często używany z CSS i JS

---

### `<span>` – kontener liniowy

```html
<p>To jest <span>ważne</span> słowo</p>
```

* działa wewnątrz tekstu
* nie łamie linii

---

## 13. Formularze – podstawy

### `<form>`

```html
<form>
  ...
</form>
```

* służy do zbierania danych od użytkownika
* współpracuje z JS i backendem

---

### `<input>`

```html
<input type="text">
<input type="password">
```

* pole do wprowadzania danych
* typ określa zachowanie pola

---

### `<button>`

```html
<button>Kliknij</button>
```

* przycisk
* często obsługiwany przez JavaScript

### Atrybut `type` w znaczniku `<button>` 

Atrybut `type` w znaczniku `<button>` **określa zachowanie przycisku**, czyli co dokładnie stanie się po jego kliknięciu. Jest to **szczególnie ważne w kontekście formularzy HTML**, ponieważ domyślne zachowanie przycisku może być inne, niż się spodziewamy.

---

### Domyślne zachowanie

Jeśli **nie podamy atrybutu `type`**, przeglądarka **automatycznie ustawi**:

```html
<button>Wyślij</button>
```

To jest równoważne:

```html
<button type="submit">Wyślij</button>
```

Czyli:

* przycisk **wysyła formularz**
* może spowodować przeładowanie strony

To jest **częste źródło błędów u studentów**.

---

### Dostępne wartości `type`

#### `type="submit"`

```html
<button type="submit">Wyślij</button>
```

* wysyła formularz
* uruchamia walidację HTML
* używany do zatwierdzania danych

---

#### `type="button"`

```html
<button type="button">Kliknij</button>
```

* **nie wysyła formularza**
* służy wyłącznie do obsługi przez JavaScript
* **najbezpieczniejszy wybór**, gdy przycisk ma coś tylko „robić”

---

#### `type="reset"`

```html
<button type="reset">Wyczyść</button>
```

* resetuje pola formularza do wartości początkowych
* rzadko używany w praktyce

---

### Najważniejsze zdanie na egzamin

> Jeśli przycisk znajduje się wewnątrz formularza i nie ma określonego `type`, to **domyślnie działa jak `submit`**.

---

### Dobra praktyka

✔ Zawsze **jawnie podawaj `type`**:

```html
<button type="button">+</button>
<button type="submit">Zapisz</button>
```

To zapobiega niekontrolowanemu wysyłaniu formularzy.