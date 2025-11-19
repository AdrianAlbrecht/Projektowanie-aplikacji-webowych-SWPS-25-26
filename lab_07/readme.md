# Projektowanie Aplikacji Webowych, semestr 2025Z

## Lab 7 - Queryset oraz budowa pierwszych endpoint'ów REST API.
---

Mamy modele już gotowe do pracy. Chcielibyśmy móc za pomocą kwerend do bazy danych móc pobrać te informacje o istniejących już danych i zacząć je obrabiać. Do tego posłuży nam klasa `QuerySet` z naszego modulu Django.

---

#### **1. Klasa QuerySet i dostępne metody:**
https://docs.djangoproject.com/pl/5.2/ref/models/querysets/

Django ORM korzysta z obiektów `QuerySet` do wykonywania zapytań do bazy danych. `QuerySet` reprezentuje kolekcję obiektów modelu i pozwala na ich filtrowanie, sortowanie oraz manipulowanie danymi. Poniżej znajdziesz przegląd najczęściej używanych metod `QuerySet`, ich zastosowanie i przykłady.

### 1. **`all()`**
   - Zwraca wszystkie obiekty modelu.
   - **Przykład**:
     ```python
     from myapp.models import Product
     products = Product.objects.all()
     ```

### 2. **`filter(**kwargs)`**
   - Zwraca obiekty spełniające podane kryteria.
   - **Przykład**:
     ```python
     active_products = Product.objects.filter(is_active=True)
     ```

### 3. **`exclude(**kwargs)`**
   - Zwraca obiekty, które nie spełniają podanych kryteriów.
   - **Przykład**:
     ```python
     inactive_products = Product.objects.exclude(is_active=True)
     ```

### 4. **`get(**kwargs)`**
   - Zwraca jeden obiekt spełniający podane kryteria. Podnosi wyjątek `DoesNotExist`, jeśli obiekt nie istnieje, lub `MultipleObjectsReturned`, jeśli istnieje więcej niż jeden.
   - **Przykład**:
     ```python
     product = Product.objects.get(id=1)
     ```

### 5. **`count()`**
   - Zwraca liczbę obiektów w `QuerySet`.
   - **Przykład**:
     ```python
     number_of_products = Product.objects.filter(is_active=True).count()
     ```

### 6. **`exists()`**
   - Sprawdza, czy `QuerySet` zawiera jakiekolwiek obiekty (zwraca `True` lub `False`).
   - **Przykład**:
     ```python
     has_active_products = Product.objects.filter(is_active=True).exists()
     ```

### 7. **`order_by(*fields)`**
   - Sortuje obiekty według podanych pól. Dodaj `-` przed nazwą pola, aby sortować malejąco.
   - **Przykład**:
     ```python
     sorted_products = Product.objects.order_by('price')
     ```

### 8. **`distinct()`**
   - Zwraca unikalne obiekty w `QuerySet`.
   - **Przykład**:
     ```python
     unique_categories = Product.objects.values('category').distinct()
     ```

### 9. **`values(*fields)`**
   - Zwraca `QuerySet` słowników, z wartościami tylko dla podanych pól.
   - **Przykład**:
     ```python
     product_names = Product.objects.values('name')
     ```

### 10. **`aggregate(**kwargs)`**
   - Zwraca słownik z wynikami agregacji na poziomie całego `QuerySet`.
   - **Przykład**:
     ```python
     from django.db.models import Sum
     total_price = Product.objects.aggregate(Sum('price'))
     ```

### 11. **`first()`**
   - Zwraca pierwszy obiekt w `QuerySet` lub `None`, jeśli `QuerySet` jest pusty.
   - **Przykład**:
     ```python
     first_product = Product.objects.all().first()
     ```

### 12. **`last()`**
   - Zwraca ostatni obiekt w `QuerySet` lub `None`, jeśli `QuerySet` jest pusty.
   - **Przykład**:
     ```python
     last_product = Product.objects.all().last()
     ```

### 13. **`update(**kwargs)`**
   - Umożliwia aktualizację wielu obiektów w `QuerySet` za pomocą jednego zapytania.
   - **Przykład**:
     ```python
     Product.objects.filter(is_active=True).update(price=F('price') * 1.1)  # Zwiększa cenę o 10%
     ```

### 14. **`delete()`**
   - Usuwa obiekty z bazy danych. Zwraca liczbę usuniętych obiektów oraz słownik informacyjny.
   - **Przykład**:
     ```python
     deleted_count, _ = Product.objects.filter(is_active=False).delete()
     ```

### 15. **`iterator()`**
   - Umożliwia iterowanie przez `QuerySet`, co jest przydatne w przypadku dużych zbiorów danych.
   - **Przykład**:
     ```python
     for product in Product.objects.iterator():
         print(product.name)
     ```

### 16. **`only(*fields)`**
   - Pobiera tylko podane pola z bazy danych, co może poprawić wydajność.
   - **Przykład**:
     ```python
     products = Product.objects.only('name', 'price').all()  # Pobiera tylko 'name' i 'price'
     ```

### 17. **`get_or_create(**kwargs)`**
   - Zwraca krotkę (obiekt, utworzone) lub zwraca istniejący obiekt, jeśli już istnieje.
   - **Przykład**:
     ```python
     product, created = Product.objects.get_or_create(name='New Product', defaults={'price': 10.0})
     ```

### 18. **`bulk_create(objs, batch_size=None)`**
   - Umożliwia szybkie tworzenie wielu obiektów w jednym zapytaniu.
   - **Przykład**:
     ```python
     products = [Product(name='Product 1'), Product(name='Product 2')]
     Product.objects.bulk_create(products)
     ```

### 19. **`bulk_update(objs, fields)`**
   - Umożliwia szybkie aktualizowanie wielu obiektów w jednym zapytaniu.
   - **Przykład**:
     ```python
     products = Product.objects.all()
     for product in products:
         product.price += 1.0
     Product.objects.bulk_update(products, ['price'])
     ```

### 20. **`update_or_create(**kwargs)`**
   - Umożliwia aktualizowanie istniejącego obiektu lub tworzenie nowego, jeśli obiekt nie istnieje.
   - **Przykład**:
     ```python
     product, created = Product.objects.update_or_create(
         name='Existing Product',
         defaults={'price': 20.0}
     )
     ```

Metody `QuerySet` w Django ORM są potężne i elastyczne, umożliwiając łatwe manipulowanie danymi w bazie danych. Wybór odpowiednich metod może znacząco poprawić wydajność aplikacji.

### **2. Wykorzystanie widoków APIView do tworzenia endpointów REST API.**

REST API (Representational State Transfer Application Programming Interface) to styl architektoniczny używany do projektowania interfejsów API, który opiera się na protokole HTTP. REST opiera się na kilku zasadach, które zapewniają prostotę, skalowalność i łatwość użycia. Poniżej znajduje się opis podstawowych zasad oraz najczęściej używanych metod żądań w REST API.

### Zasady REST API

1. **Stateless**: Każde żądanie do serwera musi zawierać wszystkie informacje potrzebne do jego zrozumienia. Serwer nie przechowuje stanu klienta między różnymi żądaniami. To oznacza, że każde żądanie jest niezależne od innych.

2. **Użycie HTTP**: REST API wykorzystuje standardowe metody HTTP, co umożliwia korzystanie z różnych protokołów i narzędzi do komunikacji.

3. **Zasoby**: W REST API zasoby są identyfikowane przez unikalne identyfikatory URI (Uniform Resource Identifier). Zasoby mogą być reprezentowane w różnych formatach, najczęściej jako JSON lub XML.

4. **Interfejs uniformny**: REST API posiada jednolity interfejs, co oznacza, że komunikacja odbywa się w określony sposób, co ułatwia zrozumienie i implementację.

5. **Klient-serwer**: Architektura REST oddziela klienta od serwera, co umożliwia niezależny rozwój obu komponentów.

6. **Cache**: Odpowiedzi z serwera mogą być buforowane, co przyspiesza działanie aplikacji i zmniejsza obciążenie serwera.

### Metody żądań w REST API

REST API korzysta z różnych metod HTTP do wykonywania operacji na zasobach. Oto najpopularniejsze metody:

1. **GET**:
   - **Opis**: Służy do pobierania zasobów z serwera.
   - **Przykład użycia**: `GET /api/books/` - pobiera listę wszystkich książek.

2. **POST**:
   - **Opis**: Używana do tworzenia nowych zasobów na serwerze.
   - **Przykład użycia**: `POST /api/books/` z danymi książki w ciele żądania, co tworzy nową książkę.

3. **PUT**:
   - **Opis**: Używana do aktualizacji istniejących zasobów. Zazwyczaj wysyła wszystkie dane zasobu, nawet te, które się nie zmieniają.
   - **Przykład użycia**: `PUT /api/books/1/` z danymi aktualizowanej książki w ciele żądania.

4. **PATCH**:
   - **Opis**: Używana do częściowej aktualizacji zasobu. Wysyła tylko te dane, które mają zostać zmienione.
   - **Przykład użycia**: `PATCH /api/books/1/` z danymi, które mają być zmienione.

5. **DELETE**:
   - **Opis**: Służy do usuwania zasobów z serwera.
   - **Przykład użycia**: `DELETE /api/books/1/` - usuwa książkę o identyfikatorze 1.

### Przykłady żądań REST API

Oto kilka przykładów żądań, które można wysłać do REST API:

- **Pobranie wszystkich książek**:
  ```http
  GET /api/books/
  ```

- **Pobranie konkretnej książki**:
  ```http
  GET /api/books/1/
  ```

- **Utworzenie nowej książki**:
  ```http
  POST /api/books/
  Content-Type: application/json

  {
      "title": "Django for Beginners",
      "author": "William S. Vincent",
      "published_date": "2021-01-01"
  }
  ```

- **Aktualizacja książki**:
  ```http
  PUT /api/books/1/
  Content-Type: application/json

  {
      "title": "Django for Beginners",
      "author": "William S. Vincent",
      "published_date": "2021-01-01"
  }
  ```

- **Częściowa aktualizacja książki**:
  ```http
  PATCH /api/books/1/
  Content-Type: application/json

  {
      "author": "W. S. Vincent"
  }
  ```

- **Usunięcie książki**:
  ```http
  DELETE /api/books/1/
  ```

> **Ważne: Aby skorzystać z widoków dla REST API dostarczanych z frameworkiem DRF należy w pliku `projekt\settings.py` dodać w `INSTALLED_APPS` wpis `rest_framework`.**

W odróżnienu od klasy `HttpRequest` z frameworka Django, DRF wykorzystuje rozszarzającą ją klasę `Request`, która jest lepiej przystosowana do obsługi żądań REST API. Wszelkie wartości takiego żądania znajdują się w zmiennej `request.data` w odróżnieniu od `request.POST` (klasa HttpRequest).
Do obsługi odpowiedzi wykorzystywana jest natomiast klasa `Response`, które w podstawowej formie zawiera wszelkie dane w surowej formie i dopiero w fazie negocjacji z klientem decyduje o ich postaci.

Chcąc stworzyć endpoint REST możemy wykorzystać dwa opakowania (ang. wrappers) z DRF:
* dekorator `@api_view` dla widoków opartych na funkcjach,
* klasę `APIView` dla widoków opartych na klasach.

Poniżej przykład implementacji endpointu opartego na widokach funkcyjnych (plik `views.py` wewnątrz struktury danej aplikacji.)

**_Listing 1_**
```python
from django.shortcuts import render
from rest_framework import status
from rest_framework.decorators import api_view
from rest_framework.response import Response
from .models import Book
from .serializers import BookSerializer

# określamy dostępne metody żądania dla tego endpointu
@api_view(['GET', "POST"])
def book_list(request):
    """
    Lista wszystkich obiektów modelu Book.
    """
    if request.method == 'GET':
        books = Book.objects.all()
        serializer = BookSerializer(books, many=True)
        return Response(serializer.data)
    
    elif request.method == 'POST':
        serializer = BookSerializer(data=request.data)
        if serializer.is_valid():
            serializer.save()
            return Response(serializer.data, status=status.HTTP_201_CREATED)

        return Response(serializer.errors, status=status.HTTP_400_BAD_REQUEST)


@api_view(['GET', 'PUT', 'DELETE'])
def book_detail(request, pk):

    """
    :param request: obiekt DRF Request
    :param pk: id obiektu Book
    :return: Response (with status and/or object/s data)
    """
    try:
        book = Book.objects.get(pk=pk)
    except Book.DoesNotExist:
        return Response(status=status.HTTP_404_NOT_FOUND)

    """
    Zwraca pojedynczy obiekt typu Book.
    """
    if request.method == 'GET':
        serializer = BookSerializer(book)
        return Response(serializer.data)

    elif request.method == 'PUT':
        serializer = BookSerializer(book, data=request.data)
        if serializer.is_valid():
            serializer.save()
            return Response(serializer.data)
        return Response(serializer.errors, status=status.HTTP_400_BAD_REQUEST)

    elif request.method == 'DELETE':
        book.delete()
        return Response(status=status.HTTP_204_NO_CONTENT)

```

Teraz należy jeszcze w klasie serializera dodać implementację metody `update`, która jest wywoływana dla metody `PUT` dla endpointu `book_detail`.
Dobre praktyki jednak mówią o tym, że powinniśmy dla każdej operacji przygotować oddzielny ednpoint, co pozwoli też na lepszą granulację uprawnień w tak stworzonym systemie. Aby dodać metodę stworzenia nowego obiektu dla danego modelu możemy to zrobić dla metody `PUT` (chociaż dobre praktyki też mówią a tym, że powinien być używany do operacji UPDATE) lub lepiej przez metodę `POST`.

**_Listing 2_**
```python
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

No ale znowu, w tym update nie zmieniliśmy nic, jest to standardowa (dziedzicząca po ModelSerializer) wersja metody `update` więc i bez tego powinno działać.

Aby całość zadziałała jak należy, niezbędne jest dodanie również odpowienich wpisów w plikach `urls.py` odpowiednich aplikacji projektu.

Przykład poniżej.

> UWAGA!!!
> Plik `urls.py` podobnie jak `serializers.py` w folderze **APLIKACJI** (nie projektu) domyślnie nie istnieje i musimy go ręcznie stworzyć.

**_Listing 3_**
```python
# plik biblioteka/urls.py

from django.urls import path, include
from . import views

urlpatterns = [
    path('books/', views.book_list),
    path('books/<int:pk>/', views.book_detail),
]
```

I przykładowy plik `gr_X_project/urls.py` z importem url'i danej aplikacji.

**_Listing 4_**
```python
from django.contrib import admin
from django.urls import path, include


urlpatterns = [
    path('admin/', admin.site.urls),
    path('biblioteka/', include('biblioteka.urls'),)
]
```

Umieszczając definicje dla każdej aplikacji wewnątrz jej struktury, uniezależniamy ją jeszcze bardziej od głównego projektu i możemy łatwiej przenosić pomiędzy projektami. Wymagane jest jeszcze dołączenie tych urli w pliku `urls.py` głównego projektu.

---

No ale znowu pojawia się pytanie, czy jesteśmy w stanie wykorzystać klasy, tak jak w przypadku Serializatorów? 

### Class Based Views w Django?

**Class Based Views (CBV)** to widoki reprezentowane jako **klasy**, a nie funkcje.
Dzięki temu możesz:

* korzystać z gotowych szablonów zachowań (listowanie, tworzenie, edycja itp.),
* łatwo rozszerzać widoki,
* korzystać z dziedziczenia,
* pisać mniej kodu.

Django dostarcza wiele gotowych CBV, m.in.:

| Widok          | Zastosowanie                   |
| -------------- | ------------------------------ |
| `ListView`     | Lista obiektów                 |
| `DetailView`   | Szczegóły pojedynczego obiektu |
| `CreateView`   | Formularz dodawania            |
| `UpdateView`   | Formularz edycji               |
| `DeleteView`   | Potwierdzenie usunięcia        |
| `TemplateView` | Render dowolnego szablonu      |

---

Poniżej mamy **gotową wersję Class Based Views (CBV)** w Django REST Framework, które w pełni zastępują nasze widoki funkcyjne.

Użyjemy:

* `ListAPIView` (lub `ListCreateAPIView`)
* `RetrieveUpdateDestroyAPIView`

To najczystszy, idiomatyczny sposób.

---

### **1. Widok listy książek – CBV**

Jeśli widok ma obsługiwać tylko **GET**, używamy `ListAPIView`.

**_Listing 5_**

```python
from rest_framework.generics import ListAPIView
from .models import Book
from .serializers import BookSerializer

class BookListView(ListAPIView):
    queryset = Book.objects.all()
    serializer_class = BookSerializer
```

---

### **2. Widok szczegółowy – GET, PUT, DELETE**

Używamy:

**`RetrieveUpdateDestroyAPIView`**

**_Listing 6_**
```python
from rest_framework.generics import RetrieveUpdateDestroyAPIView
from .models import Book
from .serializers import BookSerializer

class BookDetailView(RetrieveUpdateDestroyAPIView):
    queryset = Book.objects.all()
    serializer_class = BookSerializer
```

---

### **To wszystko!**

Dwa CBV zastępują cały poprzedni kod, łącznie z obsługą:

* pobierania obiektu,
* 404 przy braku obiektu,
* walidacji,
* zapisu,
* obsługi PUT/DELETE.

Django REST Framework robi to automatycznie.

---

### **3. URL-e (dla kompletności)**

**_Listing 7_**
```python
from django.urls import path
from .views import BookListView, BookDetailView

urlpatterns = [
    path("books/", BookListView.as_view(), name="book-list"),
    path("books/<int:pk>/", BookDetailView.as_view(), name="book-detail"),
]
```

---

### Wersja z możliwością dodawania

Jeśli chcesz również obsługi **POST /books/**:

### Zamień `ListAPIView` na:

**_Listing 8_**
```python
from rest_framework.generics import ListCreateAPIView

class BookListView(ListCreateAPIView):
    queryset = Book.objects.all()
    serializer_class = BookSerializer
```

---

# **Zadania**

**Zadanie 1**  

Wykonaj zadania na nowym branchu o nazwie `lab_07`. Na koniec pracy, po przetestowaniu, scal ten branch z główną gałęzią projektu.

**Zadanie 2 (CIĘŻSZE ZADANIE, opcjonalne, dla chętnych)**

Korzystając z dokumentacji API klasy QuerySet z pkt. 1 wykonaj następujące zapytania za pomocą shella Django (**kod Pythona z zapytaniami umieść w pliku lab_5_zadanie_6.md w swoim repozytorium**):
* wyświetl wszystkie obiekty modelu `Osoba`,
* wyświetl obiekt modelu `Osoba` z id = 3,
* wyświetl obiekty modelu `Osoba`, których nazwa rozpoczyna się na wybraną przez Ciebie literę alfabetu (tak, żeby był co najmniej jeden wynik),
* wyświetl unikalną listę stanowisk przypisanych dla modeli `Osoba`,
* wyświetl nazwy stanowisk posortowane alfabetycznie malejąco,
* dodaj nową instancję obiektu klasy `Osoba` i zapisz w bazie.

**Zadanie 3**

 Bazując na przykładach z bieżącego laboratorium przygotuj endpointy dla modeli `Osoba` i `Stanowisko`:
   * wyświetlanie, dodawanie i usuwanie pojedynczego obiektu typu `Osoba`,
   * wyświetlanie listy obiektów typu `Osoba`,
   * wyświetlenie listy obiektów typu `Osoba`, które zawierają w polu `nazwa` zadany łańcuch znaków,
   * wyświetlanie, dodawanie i usuwanie pojedynczego obiektu typu `Stanowisko`,
   * wyświetlanie listy obiektów typu `Stanowisko`.

**Zadanie 4**

 Korzystając z posiadanego API (**NIE Z ADMIN PANELU**) wykonaj:
   * dodaj dwa nowe obiekty `Osoba` eksperymentując z różnymi polami,
   * zmodyfikuj jeden obiekt typu `Osoba`,
   * usuń jeden obiekt typu `Osoba`,
   * wyświetl wszystkie obiekty, które w nazwie zawierają literę `a`.

**Zadanie 5**

 Bazując na przykładzie z dokumentacji pod adresem https://www.django-rest-framework.org/tutorial/3-class-based-views/ wykonaj:
   * zatwierdź zmiany w poprzednim branchu
   * dodaj nowy branch o nazwie `lab_7_class_views` i przełącz się na niego
   * zamień implementację API dla modelu `Osoba` zgodnie z przykładem z dokumentacji, rozszerzając klasę `APIView` DRF, przetestuj działanie.
   * zatwierdź zmiany na branchu, ale nie scalaj z główną gałęzią.

   **Zadanie 6 (nie obowiązkowe)**

 Do odpytania endpointów oprócz widoków serwowanych przez DRF wykorzystaj również program `Postman` (https://www.postman.com/downloads/).
