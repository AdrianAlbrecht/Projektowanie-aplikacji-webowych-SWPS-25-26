# Projektowanie Aplikacji Webowych, semestr 2025Z

## Lab 3

## Uzupełnienie co do git

Na ten czas mamy dane na repozytorium zdalnym i lokalnym. Ale co w przypadku, gdy chcemy pracować na tych danych w domu i rzwijać nasz projekt? Albo pracujemy w grupie? Jak sklonować te dane? Jeżeli nie mamy jeszcze tego repozytorium lokalnie u sibeie na komputerze to możemy posłużyć się poleceniem:

```bash
git clone [repository-url]
```

Albo użyć wbudowanych metod klonowania w naszym VS Code.

### Tworzenie nowych branchy

Gałęzie pozwalają na rozwijanie nowych funkcjonalności bez ingerencji w główną wersję projektu.

```bash
git branch <nazwa_gałęzi>
```

Aby przełączyć się na nową gałąź:

```bash
git checkout <nazwa_gałęzi>
```

Możesz również utworzyć i przełączyć się na gałąź w jednym kroku:

```bash
git checkout -b <nazwa_gałęzi>
```

### **Scalanie gałęzi (merge)**
Kiedy skończysz pracować na gałęzi, możesz scalić jej zmiany z główną gałęzią (zwykle `master` lub `main`).

```bash
git checkout main
git merge <nazwa_gałęzi>
```

Możemy też posłużyć się wbudowanym do tego graficznym UI w naszym VS Code.

### Pull Request

Jak pewnie zostało zauważone - ręczne mergowanie branchy jest dość kłopotliwe - musimy zaufać sobie czy komuś innemu, że wszystko jest w porządku. Na szczęście używając platformy GitHub możemy utworzyć zlecenie scalenia naszych gałęzi w bardzie kontrolowany sposób, czyli Pull Reqests.

Dzięki temu możemy nie tylko wystawić żadanie scalenia, ale przed tym scaleniem nasz GitHub sprawdzi ewentualne konflikty oraz możemy poprosić kogoś o ocenę, komentarz i ewetualne uzsykanie zgody od reszty współpracowników na scalenie. Pozwala to uniknąć niezrozumiałych scaleń oraz nierozwiązanych konfliktów w kodzie.

### Zatem w skrócie

#### Kiedy inicjujemy nowe repozytorium

Aby zinicjować nowe repozytorium, kiedy mamy już kod lokalnie, należy najpierw utworzyć nowe repozytorium zdalne (np. na platformie GitHub).

Następnie w naszym folderze lokalnie uruchomic komendę:

```bash
git init
```

Następnie musimy skonfigurować nasze dane za pomocą komend:

```bash
git config --local user.name twojaNazwaUżytkownika
git config --local user.email twojemail@twoja.domena.pl
```

Następnie utworzyć plik `.gitignore`, aby wyłączyć ze sprawdzania pliki niepotrzebne. To w nim wpisujemy nazwy plików, folderów czy wtrażenia regularne, których nie potrzebujemy w naszym repozytorium zdalnym.

Następnie, aby dodac wszystkie nowe pliki wykonujemy polecenie:

```bash
git add .
```

Następnie tworzymy migawkę poleceniem:

```bash
git commit -m "Tutaj wpisz tytuł commita :)"
```

Następnie musimy połączyć nasze repozytorium zdalne z lokalnym używając linka podanego w repo:

```bash
git remote add origin https://github.com/user/repo.git
```

Następnie musimy wypuścić zmiany do repozytoriu zdalnego (czasami wymagana jest zmiana `master` na `main` w zależności od wersji gita):

```bash
git push --set-upstream origin master
```

#### Kiedy chcemy sklonować istniejące już repozytorium zdalne z kodem

Trzeba skonfigurować sowje dane poleceniem `git config` a następnie sklonować repozytorium za pomocą:

```bash
git clone [repository-url]
```

### Teraz za każdym razem
#### Kiedy chcesz pobrać nowe zmiany z repozytorum zdalnego

Użyj komend w tej kolejności:

```bash
git fetch
git pull
```

#### Kiedy chcesz wypuścić zmiany do repozytorium zdalnego:

>Przed każdym wypuszczeniem zmian jak i w trakcie każdego rozpoczęcia pracy na repozytorium lokalnym zalecane jest wykonanie `git fetch` i `git pull`, aby być pewnym, ze pracujemy na najnowszej wersji!!!!

Użyj komend w tej kolejności:

```bash
git add .
git commit -m "Tytuł commita"
git push
```

## Nadal Enigma?

Jeżeli nadal masz problemy z gitem, zerknij tutaj: https://learngitbranching.js.org/?locale=pl

## A dzisiaj...

### **1. Narzędzia administracyjne frameworka Django.**

Django posiada narzędzie administracyjne w postaci skryptu `manage.py` oraz polecenia `django-admin` dzięki którym możemy wykonać wiele czynności administracyjnych, takich jak dodanie użytkownika administracyjnego, wykonanie migracji bazy danych, uruchomienie serwera i inne. Pełną listę poleceń można znaleźć w oficjalnej dokumentacji pod adresem: https://docs.djangoproject.com/pl/4.2/ref/django-admin/

Dodatkowe i przydatne narzędzia można dodać poprzez instalację zewnętrznych modułów, np. [django-admin-tools](https://github.com/django-admin-tools/django-admin-tools) (**archiwalnie istniał problem z instalacją z Django 4.1**),[django-debug-toolbar](https://django-debug-toolbar.readthedocs.io/en/latest/index.html) lub [django-extensions](https://pypi.org/project/django-extensions/).

Jeżeli w trakcie poprzednich zajęć wykonana została tylko pierwsza część oficjalnego tutoriala, to aby móc wykonać ćwiczenia i zadania dla bieżącego laboratorium należy wykonać poniższe czynności.

**Stworzenie schematu bazy danych**

>**Uwaga** Jak zostało już wspomniane na pierwszych zajęciach praca w trakcie zajęć powinn iść dwutorowo, tj. potrzebne są dwa projekty Django, jeden do wykonania zadań w trakcie zajęć na równi z prowadzącym (co nie będzie sprawdzane, ale jest zalecane w celach edukacyjnych) oraz jako środowisko eksperymentalne, a drugi projekt to projekt na zaliczenie (grupowo). Od studenta zależy czy będzie to jedno dobrze zorganizowane repozytorium czy dwa oddzielne, jednak zalecane są oddzielne. Będzie to również miało wpływ na przygotowanie odpowiedniej konfiguracji połączenia z bazą danych przed wykonaniem operacji `migrate` (patrz dokumentacja: https://docs.djangoproject.com/en/4.2/ref/databases/).
Istnieje również dość ryzykowna opcja polegająca na stworzeniu oddzielnych aplikacji wewnątrz projektu, z której jedna będzie aplikacją testową, a druga docelowym projektem, którymi dodatkowo można zarządzać z poziomu oddzielnych gałęzi repozytorium. Jest to jednak rozwiązanie dość problematyczne, a w szczególności wymagające dużej uwagi w trakcie przełączania między nimi.

Zalecane jest korzystać z bazy danych SQLite, gdyż nie będzie sprawiać to problemów w trakcie prezentacji projektu na zaliczenie. Oraz nie potrzebujemy żadnych zewnętrznych oprogramowań dodatkowych.

Poniższa operacja stworzy schematy dla wszystkich zainstalowanych aplikacji (zmienna `INSTALLED_APPS` w pliku `settings.py`). Jeżeli nie są niezbędne to można wybrane linie zakomentować lub dodać na końcu polecenia nazwy aplikacji, dla których chcemy stworzyć schemat.

```console
# przygotowanie plików migracji
python manage.py makemigrations

# wykonanie migracji
python manage.py migrate
```

Warto przeczytać dokumentację dotyczącą tego polecenia, gdyż posiada ciekawe opcje, dość często potrzebne w trakcie początkowej pracy z modelem bazy danych i częstymi migracjami: https://docs.djangoproject.com/pl/5.2/ref/django-admin/#migrate


**Stworzenie superużytkownika**

```console
python manage.py createsuperuser
```

Ten użytkownik jest niezbędny, aby możliwe było zalogowanie się do panelu administracyjnego i wykonywanie w nim czynności administracyjnych.

Zaloguj się do panelu administracyjnego i sprawdź dostępne w nim opcje na tym etapie tworzenia aplikacji.
Adres panelu przy domyślnych ustawieniach to: http://127.0.0.1:8000/admin

### 2. Schemat bazy danych.

Każdy projekt potrzebuje jakiejś formy trwałego przechowywania choćby niewielkiej ilości danych, ale większość z nich przewiduje również powiększanie tych zbiorów przez aktywność użytkowników i rozwój aplikacji. Jeżeli nie dokonałeś jeszcze wyboru to zastanów się nad docelowym silnikiem bazy danych.

Tworzenie schematu bazy na potrzeby projektu Django można przeprowadzić na dwa sposoby.

**Sposób 1.**
Tworzymy odpowiednie implementacje klasy `django.db.models.Model` z API Django (patrz **listing 1**), a następnie poprzez migrację tworzymy schemat w docelowej bazie.

**Sposób 2**
Iżynieria wsteczna (ang. reverse engineering). Schemat bazy danych możemy przygotować bezpośrednio poprzez polecenia SQL lub narzędzia graficzne, a następnie poleceniem `manage.py inspectdb` wygenerować kod w języku Python zgodny z deklaracją, którą należy przygotować postępując sposobem pierwszym. Ten kod zostanie wyświetlony w konsoli i można strumień przekierować do pliku. Przykład poniżej.

```console
# dopisanie do pliku
manage.py inspectdb >> models.py 
```

### 3. Modele w Django.


> Dokumentacja: https://docs.djangoproject.com/pl/5.2/topics/db/models/

Definicje modeli dodajemy w pliku `projekt/aplikacja/models.py`.

__*Listing 1:*__
```python

from django.db import models

# Lista wyboru miesięcy wydania
MONTHS = models.IntegerChoices(
    'Miesiace',
    'Styczeń Luty Marzec Kwiecień Maj Czerwiec Lipiec Sierpień Wrzesień Październik Listopad Grudzień'
)

# Lista wyboru formatu książki
BOOK_FORMATS = (
    ('P', 'Papierowa'),
    ('E', 'E-book'),
    ('A', 'Audiobook'),
)


class Genre(models.Model):
    """Model reprezentujący gatunek literacki."""
    name = models.CharField(max_length=50)
    description = models.TextField(blank=True, help_text="Krótki opis gatunku literackiego.")
    typical_themes = models.CharField(
        max_length=200,
        blank=True,
        help_text="Typowe motywy i tematy występujące w tym gatunku."
    )
    is_fiction = models.BooleanField(default=True, help_text="Czy gatunek jest fikcyjny (literatura piękna).")
    popularity_rank = models.PositiveSmallIntegerField(
        default=0,
        help_text="Ocena popularności (0–10) według bibliotekarzy lub czytelników."
    )

    def __str__(self):
        return self.name


class Author(models.Model):
    """Model reprezentujący autora książki."""
    first_name = models.CharField(max_length=50)
    last_name = models.CharField(max_length=50)
    country = models.CharField(max_length=2, help_text="Kod kraju, np. PL, US, GB")

    def __str__(self):
        return f"{self.first_name} {self.last_name}"


class Book(models.Model):
    """Model reprezentujący książkę w bibliotece."""
    title = models.CharField(max_length=100)
    publication_month = models.IntegerField(choices=MONTHS.choices, default=MONTHS.Styczeń)
    book_format = models.CharField(max_length=1, choices=BOOK_FORMATS, default='P')
    author = models.ForeignKey(Author, null=True, blank=True, on_delete=models.SET_NULL)
    genre = models.ForeignKey(Genre, null=True, blank=True, on_delete=models.SET_NULL)
    available_copies = models.PositiveIntegerField(default=1)

    def __str__(self):
        return self.title
```

Po zdefiniowaniu modelu należy dodać nasza aplikację do zmiennej `INSTALLED_APPS` w pliku `settings.py`:

```python
INSTALLED_APPS = [
    'myapp.apps.MyappConfig', # <==== Tu jest nasza apka, gdzie "myapp" to nazwa naszej apki, "apps" to plik "apps.py", a "MyappConfig" to nazwa klasy konfiguracyjnej zawierającej się w pliku "apps.py"
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
]
```

przygotować migracje poleceniem `manage.py makemigrations`, a nstępnie wykonać migrację poprzez `manage.py migrate`.

Wykonanie polecenia migracji powinno propagować powyższe modele na schemat domyślnej bazy danych zdefiniowanej w pliku `settings.py`.

Warto zarejestrować od razu nasze modele w `admin.py` znajdującym się w folderze naszej aplikacji:

```python
from .models import Genre, Author, Book

admin.site.register(Genre)
admin.site.register(Author)
admin.site.register(Book)
```

**ZADANIA**

1. **PROJEKT** Zastanów się nad sposobem organizacji swojego kodu dla aplikacji testowej (zajęcia) oraz projektu docelowego i przygotuj środowisko w odpowiedni sposób.
2. Przeanalizuj plik z kodem modeli i dodaj/zmodyfikuj jedno z pól modelu. Przygotuj plik migracji (`makemigrations`), a następnie wykonaj migrację i sprawdź czy zmiany zostały propagowane na bazę. Przejrzyj zawartość pliku migracji.
3. Za pomocą polecenia `showmigrations` wylistuj wszystkie migracje dla danej aplikacji, a następnie sprawdzając w dokumentacji działanie polecenia `migrate` wycofaj ostatnią migrację.
6. Posługując się przykładem z [oficjalnego tutoriala numer 2](https://docs.djangoproject.com/pl/5.2/intro/tutorial02/) dodaj jeden model do panelu administracyjnego i przetestuj dodanie, modyfikację i usunięcie instancji tego modelu.
7. Zapisz zmiany w repozytorium i jeżeli jest to wymagane wyślij informacje o dodaktowym repozytorium dla prowadzącego zajęcia.


### Dodatkowe narzędzia i materiały.

* Do obsługi baz danych SQLite (plik w projekcie o nazwie `db.sqlite3`) może przydać się darmowe narzędzie w formie rozszerzenia do VS Code o nazwie `SQLite Viewer`, które posiada możliwość przeglądania scheamtu bazy danych, tabel oraz ich zawartości poprzez graficzne UI. Więcej: https://marketplace.visualstudio.com/items?itemName=qwtel.sqlite-viewer

