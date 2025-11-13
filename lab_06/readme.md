# Projektowanie Aplikacji Webowych, semestr 2025Z

## Lab 6 - Django Rest Framework i serializacja danych. Podstawowa walidacja wewnątrz serializerów danych.
---

### 1. Serializatory

W tradycyjnej aplikacji Django dane najczęściej przesyłane są pomiędzy **modelem**, **formularzem** i **szablonem HTML**.
Jednak w aplikacjach typu **API (Application Programming Interface)**, gdzie komunikacja odbywa się przez sieć (np. JSON lub XML), potrzebujemy narzędzia, które potrafi:

1. **Zamienić obiekt Django (np. model)** na dane możliwe do przesłania przez sieć — np. w formacie JSON, XML lub innego typu.
2. **Odwrotnie — przyjąć dane z zewnątrz (np. z żądania HTTP POST)** i przekształcić je w postać zrozumiałą dla Django (czyli np. utworzyć lub zaktualizować obiekt modelu).

Tę rolę pełnią właśnie **serializatory**.
---

#### Dlaczego jest to ważne?

Serializatory są kluczowym elementem każdego REST API, ponieważ:

| Funkcja                           | Opis                                                                                                                             |
| --------------------------------- | -------------------------------------------------------------------------------------------------------------------------------- |
| 💬 **Komunikacja**                | Umożliwiają wymianę danych między backendem Django a frontendem (np. React, Vue) lub aplikacjami mobilnymi.                      |
| 🧩 **Bezpieczeństwo i walidacja** | Sprawdzają, czy dane przesyłane przez użytkownika są poprawne i zgodne z oczekiwaniami.                                          |
| ⚙️ **Automatyzacja**              | Dzięki `ModelSerializer` można błyskawicznie stworzyć pełne API CRUD dla modelu bez pisania dużej ilości kodu.                   |
| 🔍 **Elastyczność**               | Można tworzyć różne serializatory dla tego samego modelu — np. szczegółowy (`DetailedSerializer`) i skrócony (`ListSerializer`). |

---

`Serializer` pozwala na konwersję złożonych danych na rodzime typy danych w języku Python, które można następnie łatwo przekształcić w JSON, XML lub inne typy treści. Serializatory zapewniają również deserializację, umożliwiając przekształcenie przeanalizowanych danych z powrotem w złożone typy po uprzednim sprawdzeniu poprawności danych przychodzących.

**Serializator** to element Django REST Framework, który:

* „pakuje” dane Python/Django w format JSON (lub inny) → *serializacja*
* „rozpakowuje” dane JSON na obiekty Django → *deserializacja*
* zapewnia **walidację danych** (czyli sprawdza, czy dane są poprawne zanim trafią do bazy)
* umożliwia **automatyczne tworzenie** i **aktualizowanie** obiektów modeli


`Serializer` w środowisku REST działa bardzo podobnie do klas `Form` i `ModelForm` w Django. W naszych projektach będziemy korzystać z:

- `Serializer` - ogólny sposób kontrolowania requestów,
- `ModelSerializer` - sprawdzanie modeli, inicjalizacja.

Po dokładne informacje o serializacji odsyłamy do informacji z wykładu (🙂) oraz [dokumentacji DRF](https://www.django-rest-framework.org/api-guide/serializers/).

Aby móc zacząć działać z naszym serializatorami, z racji że serializacja jest bezpośrednio związana z REST'em, to musimy dosinstalować `djangorestframework` za pomocą komendy:
```shell
pip install djangorestframework
```

Właśnie z tego modułu będziemy importować zachowania serializatorów, które będą nam potrzebne w inicjalizacji naszych serializatorów do każdego modelu.

Gdy mam już zainstalowany ten moduł możemy zacząć kodować. Nasze serializatory będziemy umieszczać w folderze aplikacji (w naszym wypadku *biblioteka*) w pliku `serializers.py`. Domyślnie niestety tego pliku nie ma, zatem musimy go sami stworzyć.

>**!!!!!!! UWAGA !!!!!!!!!!**
>
> W Django zaleca się definiowanie serializerów w pliku o nazwie **`serializers.py`** w aplikacji, w której znajdują się modele, które będą serializowane. Plik ten organizuje kod i sprawia, że jest on łatwy do znalezienia oraz utrzymania, podobnie jak pliki `models.py` dla modeli czy `views.py` dla widoków.
>
> ### Struktura pliku `serializers.py`
>
>W typowej aplikacji Django plik `serializers.py` będzie zawierał importy oraz klasy serializerów dla modeli w tej aplikacji. 
>
>### Korzyści z użycia `serializers.py`
>
>- **Organizacja**: Ułatwia zarządzanie kodem, szczególnie gdy aplikacja rośnie.
>- **Łatwiejsze testowanie i debugowanie**: Wszystkie serializery są w jednym miejscu, więc łatwiej jest je przetestować lub zaktualizować.
>- **Konwencja i czytelność**: Inni programiści pracujący nad projektem zrozumieją, że serializery można znaleźć w pliku `serializers.py`, co przyspiesza współpracę.
>
>### Kiedy dodawać serializery do innych plików?
>
>W dużych projektach, gdzie liczba serializerów jest bardzo duża, można podzielić serializery na kilka plików lub folderów. Wtedy można stworzyć katalog `serializers/` i tam umieścić pliki o nazwach odpowiadających logice aplikacji, np.:
>
>```
>myapp/
>├── serializers/
>│   ├── article_serializers.py
>│   ├── author_serializers.py
>│   └── __init__.py
>```
>
>W pliku `__init__.py` można zaimportować i udostępnić wszystkie serializery, np.:
>
>```python
># myapp/serializers/__init__.py
>from .article_serializers import ArticleSerializer
>from .author_serializers import AuthorSerializer
>```
>
>Dzięki temu zachowasz czytelność i strukturę projektu, a jednocześnie unikniesz zbyt dużych, trudnych do zarządzania plików.

## Przykład klasy do serializacji danych (dziedziczącej po klasie Serializer)

Spróbujmy zatem napisać serializer do jednego z naszych modeli - `Book`:

**__Listing 1__**
```python
from rest_framework import serializers
from .models import Book, Author, Genre, MONTHS, BOOK_FORMATS


class BookSerializer(serializers.Serializer):
    """Serializer dla modelu Book."""

    # pole tylko do odczytu — ID książki
    id = serializers.IntegerField(read_only=True)

    # pole wymagane — tytuł książki
    title = serializers.CharField(required=True)

    # pole mapowane z klasy modelu, z podaniem wartości domyślnych
    # zwróć uwagę na zapisywaną wartość do bazy dla default={wybór}[0] oraz default={wybór}[0][0]
    # w pliku models.py BOOK_FROMATS oraz MONTHS zostały wyniesione jako stałe do poziomu zmiennych skryptu
    # (nie wewnątrz modelu)

    # pole z wyborem miesiąca publikacji
    # MONTHS.choices pochodzi z modelu (Enum lub lista krotek)
    publication_month = serializers.ChoiceField(choices=MONTHS.choices, default=MONTHS.choices[0][0])

    # pole z wyborem formatu książki (np. Paperback, Hardcover, eBook)
    # BOOK_FORMATS zostało zdefiniowane jako stała w models.py
    book_format = serializers.ChoiceField(choices=BOOK_FORMATS, default=BOOK_FORMATS[0][0])

    # odzwierciedlenie pola w postaci klucza obcego
    # przy dodawaniu nowego obiektu możemy odwołać się do istniejącego poprzez inicjalizację nowego obiektu
    # np. author=Author({id}) lub wcześniejszym stworzeniu nowej instancji tej klasy
    # klucz obcy — autor książki (może być null)
    author = serializers.PrimaryKeyRelatedField(queryset=Author.objects.all(), allow_null=True)

    # klucz obcy — gatunek książki (może być null)
    genre = serializers.PrimaryKeyRelatedField(queryset=Genre.objects.all(), allow_null=True)

    # liczba dostępnych kopii książki
    available_copies = serializers.IntegerField(default=1)

    # przesłonięcie metody create() z klasy serializers.Serializer
    # metoda create() — tworzenie nowego obiektu Book
    def create(self, validated_data):
        return Book.objects.create(**validated_data)

    # przesłonięcie metody update() z klasy serializers.Serializer
    # metoda update() — aktualizacja istniejącego obiektu Book
    def update(self, instance, validated_data):
        instance.title = validated_data.get('title', instance.title)
        instance.publication_month = validated_data.get('publication_month', instance.publication_month)
        instance.book_format = validated_data.get('book_format', instance.book_format)
        instance.author = validated_data.get('author', instance.author)
        instance.genre = validated_data.get('genre', instance.genre)
        instance.available_copies = validated_data.get('available_copies', instance.available_copies)
        instance.save()
        return instance
```

Przetestowanie działania serializera możemy również przeprowadzić z poziomu shella Django wpisująć polecenie:

```shell
python manage.py shell
```


 Przykład kolejnych operacji poniżej.

_**Listing 2**_
```python

from biblioteka.models import Book
from biblioteka.serializers import BookSerializer
from rest_framework.renderers import JSONRenderer
from rest_framework.parsers import JSONParser
import io

# 1. stworzenie nowej instancji klasy Book (opcjonalne, mamy panel admin do tego również)
book = Book(title='Zbrodnia i kara', publication_month=1, available_copies=3)
# utrwalenie w bazie danych
book.save()

# 2. inicjalizacja serializera
serializer = BookSerializer(book)
serializer.data
# output - natywny typ danych Pythona (dictionary)
{'id': 16, 'name': 'Adam', 'shirt_size': ('S', 'Small'), 'miesiac_dodania': 1, 'team': None}

# 3. serializacja danych do formatu JSON
content = JSONRenderer().render(serializer.data)
content

# output
# b'{"id":1,"title":"Zbrodnia i kara","publication_month":1,"book_format":"P","author":null,"genre":null,"available_copies":3}'
# w takiej formie możemy przesłać obiekt (lub cały graf obiektów) przez sieć i po "drugiej stronie" dokonać deserializacji odtwarzając graf i stan obiektów

import io

stream = io.BytesIO(content)
data = JSONParser().parse(stream)

# tworzymy obiekt dedykowanego serializera i przekazujemy sparsowane dane
deserializer = BookSerializer(data=data)
# sprawdzamy, czy dane przechodzą walidację (aktualnie tylko domyślna walidacja, dedykowana zostanie przedstawiona na kolejnych zajęciach)
deserializer.is_valid()
# output
# True

# to oznacza brak pojawienia się błędu walidacji
# Gdybyśmy jednak zmienili wartość argumentu `null` na False przy polu author, wtedy powstał by problem, który możemy wyświelić następującym kodem
deserializer.errors
# Nasz output
# {}
# Ale gdyby był błąd mógłby wyglądać następująco
# {'author': [ErrorDetail(string='Pole nie może mieć wartości null.', code='null')]}


# aby upewnić się w jaki sposób wyglądają pola wczytanego serializera/deserializera, możemy wywołać zmienną deserializer.fields, aby wyświetlić te dane
deserializer.fields

# lub
repr(deserializer)

# możemy sprawdzić jak wyglądają dane obiektów po deserializacji i walidacji
deserializer.validated_data
# output
# {'title': 'Zbrodnia i kara',
# 'publication_month': 1,
# 'book_format': 'P',
# 'author': None,
# 'genre': None,
# 'available_copies': 3}

# oraz utrwalamy dane
deserializer.save()
# output
# <Book: Zbrodnia i kara>
# sprawdzamy m.in. przyznane id
deserializer.data
# {'id': 2, 'title': 'Zbrodnia i kara', 'publication_month': 1, 'book_format': 'P', 'author': None, 'genre': None, 'available_copies': 3}
```

Aby wyjść z `django shell` należy wpisać w kolejnej linii:

```shell
quit()
```

W przykładzie powyżej widać sporo nadmiarowej pracy w stosunku do zdefiniowanych wcześniej modeli (możemy oczywiście chcieć serializować również inne obiekty niż modele z naszego projektu) i na pewno pojawiła się refleksja  - "Czy można wykorzystać jakąś część kodu z klas modeli?". Otóż można, wykorzystując klasę `ModelSerializer` z Django Rest Framework.


## Przykład klasy do serializacji danych (dziedziczącej po klasie ModelSerializer)

Dokumentacja: https://www.django-rest-framework.org/api-guide/serializers/#modelserializer

_**Listing 3**_
```python
class BookSerializer(serializers.ModelSerializer):
    class Meta:
        # musimy wskazać klasę modelu
        model = Book
        # definiując poniższe pole możemy określić listę właściwości modelu,
        # które chcemy serializować
        fields = ['id', 'title', 'publication_month', 'book_format', 'author', 'genre', 'available_copies']
        # definicja pola modelu tylko do odczytu
        read_only_fields = ['id']
```

Powyższa klasa serializera wykorzystuje wszystkie własności pól z klasy modelu, co znacznie zmniejsza ilość powielanego kodu i redukuje czas i ilość pracy niezbędny do dokonania zmian walidacji pól modelu. Te cechy zostaną pobrane do serializera z definicji modelu, więc nie musimy ich przechowywać w dwóch miejscach.

Testowanie kodu odbywa się adekwatnie do przykładu z serializers.Serializer, czyli *Listing nr 2*.

### **2. Walidacja danych w procesie serializacji.**

#### **2.1 Walidacja na poziomie pojedynczego pola.**

Oprócz automatycznej walidacji wartości pól na podstawie wybranego typu pola (numeryczne, tekstowe, daty itd.) możliwe jest również zdefiniowanie reguł walidacji, które są nieco bardziej złożone lub specyficzne dla danego problemu biznesowego. Aby automatycznie przypisać taki walidator dla konkretnego pola musimy jego nazwę zdefiniowac wg. wzorca `.validate_<nazwa_pola>` wewnątrz serializera. Metoda ta przyjmuje pojedynczy argument, który jest wartością pola, które ma zostać poddana walidacji. Ta metoda zwraca zwalidowaną wartość pola lub zgłasza wyjątek `serializers.ValidationError`. Przykład poniżej.

**_Listing 4_**
```python
# fragment klasy BookSerializer

# walidacja wartości pola title
    def validate_title(self, value):
        if not value.istitle():
            raise serializers.ValidationError(
                "Tytuł książki powinien rozpoczynać się wielką literą!"
            )
        return value
```

Jeżeli walidacja danego pola nie powiedzie się to zmienna `.errors` przechowa stosowny komunikat o błędzie i nie pozwoli na zapisanie obiektu przed usunięciem wszystkich błędów.

#### **2.2 Walidacja na poziomie obiektu.**

Walidacja na poziomie obiektu jest potrzebna, kiedy niezbędne jest wykorzystanie dostępu do wielu pól. Przykład (z oficjalnej dokumentacji) poniżej.

**_Listing 5_**
```python
class AuthorSerializer(serializers.ModelSerializer):
    class Meta:
        model = Author
        fields = '__all__'

    def validate(self, data):
        """
        Walidacja całego obiektu autora.
        Sprawdza poprawność formatu imienia, nazwiska i kodu kraju.
        """
        first_name = data.get('first_name')
        last_name = data.get('last_name')
        country = data.get('country')

        # Imię i nazwisko powinny zaczynać się wielką literą
        if first_name and not first_name.istitle():
            raise serializers.ValidationError(
                {"first_name": "Imię powinno rozpoczynać się wielką literą!"}
            )

        if last_name and not last_name.istitle():
            raise serializers.ValidationError(
                {"last_name": "Nazwisko powinno rozpoczynać się wielką literą!"}
            )

        # Kod kraju: dokładnie 2 wielkie litery
        if country and (len(country) != 2 or not country.isupper()):
            raise serializers.ValidationError(
                {"country": "Kod kraju musi składać się z 2 wielkich liter, np. 'PL'."}
            )

        return data
```

#### **2.3 Własne i wbudowane walidatory.**

W przypadku gdy nasze reguły walidacji (oprócz już tych wbudowanych we frameworku) trzeba wykorzystać w wielu polach i wielu serializerach, najlepszym pomysłem jest zdefiniować je jako zewnętrzne funkcje lub obiekty. Można to zrobić wewnątrz pliku z kodem serializerów, ale jeszcze lepszym pomysłem będzie wyniesienie ich do oddzielnego modułu (pliku). Poniżej przykład dla pierwszego przypadku (również z oficjalnej dokumentacji DRF).

**_Listing 6_**
```python
# metoda walidująca, można stworzyć oddzielny moduł z wieloma takimi metodami 
# i zaimportować w różnych miejscach projektu
def multiple_of_two(value):
    if value % 2 != 0:
        raise serializers.ValidationError("Ocena popularności musi być wielokrotnością 2 (np. 0, 2, 4, 6, 8, 10).")


class GenreSerializer(serializers.ModelSerializer):
    popularity_rank = serializers.IntegerField(validators=[multiple_of_two])
    
    class Meta:
        model = Genre
        fields = "__all__"
```

DRF posiada również wbudowane walidatory, które mogą służyć do walidacji np. unikalności wartości w danym zbiorze (np. tabeli w bazie danych). Przykład jego wykorzystania poniżej.

**_Listing 7_**
```python
from rest_framework.validators import UniqueTogetherValidator
```
```python
class AuthorSerializer(serializers.ModelSerializer):
    class Meta:
        model = Author
        fields = '__all__'

        validators = [
            UniqueTogetherValidator(
                queryset=Author.objects.all(),
                fields=['first_name', 'last_name']
            )
        ]
```

Django oferuje wiele wbudowanych walidatorów (Lista oraz przykłady wykorzystania tych walidatorów znajdują się w dokumentacji pod adresem: https://www.django-rest-framework.org/api-guide/validators/), które pomagają w zapewnieniu, że dane wprowadzone przez użytkowników są zgodne z określonymi regułami i standardami. Oto lista niektórych z najpopularniejszych walidatorów, ich zastosowanie oraz przykłady użycia:

### 1. `MaxLengthValidator`
- **Opis**: Sprawdza, czy długość pola nie przekracza określonej wartości.
- **Przykład użycia**:
  ```python
  from django.core.validators import MaxLengthValidator
  from django.db import models

  class ExampleModel(models.Model):
      name = models.CharField(max_length=100, validators=[MaxLengthValidator(50)])
  ```

### 2. `MinLengthValidator`
- **Opis**: Sprawdza, czy długość pola jest większa lub równa określonej wartości.
- **Przykład użycia**:
  ```python
  from django.core.validators import MinLengthValidator
  from django.db import models

  class ExampleModel(models.Model):
      username = models.CharField(max_length=30, validators=[MinLengthValidator(5)])
  ```

### 3. `EmailValidator`
- **Opis**: Sprawdza, czy wartość pola jest poprawnym adresem e-mail.
- **Przykład użycia**:
  ```python
  from django.core.validators import EmailValidator
  from django.db import models

  class ExampleModel(models.Model):
      email = models.EmailField(validators=[EmailValidator()])
  ```

### 4. `URLValidator`
- **Opis**: Sprawdza, czy wartość pola jest poprawnym adresem URL.
- **Przykład użycia**:
  ```python
  from django.core.validators import URLValidator
  from django.db import models

  class ExampleModel(models.Model):
      website = models.CharField(max_length=200, validators=[URLValidator()])
  ```

### 5. `RegexValidator`
- **Opis**: Umożliwia walidację pola przy użyciu wyrażenia regularnego.
- **Przykład użycia**:
  ```python
  from django.core.validators import RegexValidator
  from django.db import models

  class ExampleModel(models.Model):
      phone_number = models.CharField(
          max_length=15,
          validators=[RegexValidator(regex=r'^\+?1?\d{9,15}$')]
      )
  ```

### 6. `MinValueValidator`
- **Opis**: Sprawdza, czy wartość pola jest większa lub równa określonej wartości minimalnej.
- **Przykład użycia**:
  ```python
  from django.core.validators import MinValueValidator
  from django.db import models

  class ExampleModel(models.Model):
      age = models.IntegerField(validators=[MinValueValidator(18)])
  ```

### 7. `MaxValueValidator`
- **Opis**: Sprawdza, czy wartość pola nie przekracza określonej wartości maksymalnej.
- **Przykład użycia**:
  ```python
  from django.core.validators import MaxValueValidator
  from django.db import models

  class ExampleModel(models.Model):
      rating = models.IntegerField(validators=[MaxValueValidator(5)])
  ```

### 8. `ValidationError`
- **Opis**: Umożliwia zdefiniowanie własnych warunków walidacji i zgłaszanie błędów, gdy warunki nie są spełnione.
- **Przykład użycia**:
  ```python
  from django.core.exceptions import ValidationError
  from django.db import models

  def validate_even(value):
      if value % 2 != 0:
          raise ValidationError(f'{value} is not an even number.')

  class ExampleModel(models.Model):
      even_number = models.IntegerField(validators=[validate_even])
  ```

### 9. `FileExtensionValidator`
- **Opis**: Sprawdza, czy rozszerzenie pliku jest dozwolone.
- **Przykład użycia**:
  ```python
  from django.core.validators import FileExtensionValidator
  from django.db import models

  class ExampleModel(models.Model):
      file = models.FileField(validators=[FileExtensionValidator(allowed_extensions=['pdf', 'docx'])])
  ```

### 10. `BooleanValidator`
- **Opis**: Sprawdza, czy wartość pola jest wartością logiczną.
- **Przykład użycia**:
  ```python
  from django.core.validators import BooleanValidator
  from django.db import models

  class ExampleModel(models.Model):
      is_active = models.BooleanField(validators=[BooleanValidator()])
  ```

### 11. `MaxLengthValidator`
- **Opis**: Waliduje maksymalną długość pola tekstowego.
- **Przykład użycia**:
  ```python
  from django.core.validators import MaxLengthValidator
  from django.db import models

  class ExampleModel(models.Model):
      description = models.CharField(max_length=255, validators=[MaxLengthValidator(100)])
  ```


# Zadania

Celem ćwiczeń będzie stworzenie klas odpowiedzialnych za serializację danych w naszym projekcie API. Następujące polecenia powinny być wykonane dla każdego modelu w projekcie, do którego będziemy mieli dostęp zewnętrzny (przez dostęp do endpointu, np. dodanie nowej osoby do systemu).


**Zadanie 1.** 

Tworzymy nowy branch na potrzeby tego labu.

**Zadanie 2.** 

Instalujemy moduł `djangorestframework` do naszego środowiska ( o ile go jeszcze nie ma ).

**Zadanie 3.** 

W **aplikacji**, w której znajduje się nasz model otwieramy/tworzymy plik `serializers.py`.

**Zadanie 4.**

Napisz 1 klasę serializera dla swojej aplikacji dziedziczącą po klasie `serializers.Serializer`.

**Zadanie 5.** 

Resztę modeli zaimplementuj w postaci serializerów `ModelSerializer` jeżeli to możliwe.

**Zadanie 6**

Dodaj również walidację dla klasy Osoba:
   * `imię` oraz `nazwisko` - może zawierać tylko litery, gdzie pierwsza litera musi być duża

**Zadanie 7. (CIĘŻSZE ZADANIE, opcjonalne, dla chętnych)** 

Napisz kod prezentujący wykorzystanie wybranych dwóch serializerów (patrz listing 2) ze swojej aplikacji i umieść go w pliku markdown o nazwie `drf_serializer_test.md` w folderze `./aplikacja/docs/` (utwórz folder `docs`).

>Do konsoli django można również przekazać cały plik z kodem testującym przekazując go jako potok wejściowy:
>```console
>python manage.py shell < ./sciezka/do_pliku.py
>```
>Należy jednak pamiętać o tym, że w ten sposób musimy wykonywac importy z podaniem nazwy aplikacji np. `from biblioteka.models import Book`, a nie `from .models import Book`.
>
>Minusem jest niestety dość nieczytelny output, gdzie znaczniki wyjścia konsoli Pythona (czyli >>>) mogą się wielokrotnie powtarzać.


## Dodatkowe wskazówki:

* Nadpisujemy odpowiednie funkcje `create`, `update` itp. **TYLKO** w momencie gdy musimy zapisać obiekty w inny sposób (np. chcemy dodać aktualnie zalogowanego użytkownika jako właściciela stworzonego modelu, chcemy zamienić wielkość znaków, usunąć jakieś znaki z tekstu itp.).
