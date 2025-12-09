# Projektowanie Aplikacji Webowych, semestr 2025Z

## Lab 9 - Autentykacja w Django i DRF.
---

### **1. Autentykacja (uwierzytelnianie).**

**Autentykacja** to proces polegający na potwierdzeniu tożsamości (uwierzytelnianie) klienta (aplikacji), który został wcześniej zarejestrowany w danym systemie. **Autoryzacja** to z kolei proces sprawdzenia uprawnień dla zasobów, do których klient próbuje uzyskać dostęp.

W przypadku Django i oraz DRF istnieje kilka sposobów na implementację procesu uwierzytelniania. W tym labie zostaną przedstawione tylko dwie z nich: uwierzytelnianie na podstawie sesji oraz tokena.

>**Oficjalna dokumentacja przydatna przy zadaniach z bieżącego labu:**
>* tutorial: https://www.django-rest-framework.org/tutorial/4-authentication-and-permissions/
>* Authentication API Reference: https://www.django-rest-framework.org/api-guide/authentication/
>* Permissions API Reference: https://www.django-rest-framework.org/api-guide/permissions/

Do tej pory dostęp do API nie jest w żaden sposób kontrolowany i każdy znając adres URL może nasze API odpytać, również dla żądań PUT, POST czy DELETE.
Aby dla danego endpointu określić restrykcje polegającą na dostępie tylko dla zalogowanego użytkownika musimy dokonać kilku zmian w dotychczasowym projekcie.

**Krok 1 - przebudowanie endpointu dla klasy Book.**

Przyjmijmy, że żądanie GET będzie możliwe bez restrykcji, a pozostałe będą wymagały uwierzytelnionego użytkownika.
Aktualnie wszystkie metody (PUT, GET, DELETE) są zdefiniowane w jednej funkcji, co utrudnia nam możliwość rozdzielenia uprawnień dla każdego z nich.
Rozdzielimy więc oba widoki. Dla przypomnienia, poniżej oryginalny kod dla endpointów związanych z klasą Book.

**_Listing 1_**
 ```python
from django.shortcuts import render
from rest_framework import status
from rest_framework.decorators import api_view
from rest_framework.response import Response
from .models import Book
from .serializers import BookSerializer

# określamy dostępne metody żądania dla tego endpointu
@api_view(['GET'])
def book_list(request):
    """
    Lista wszystkich obiektów modelu Book.
    """
    if request.method == 'GET':
        books = Book.objects.all()
        serializer = BookSerializer(books, many=True)
        return Response(serializer.data)


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

Rozdzielamy więc metodę GET od reszty i dodajemy brakujące importy oraz dekoratory dla żadań PUT i DELETE.

**_Listing 2_**

```python
from django.shortcuts import render
from rest_framework import status
from rest_framework.decorators import api_view, authentication_classes, permission_classes
from rest_framework.authentication import SessionAuthentication, BasicAuthentication
from rest_framework.permissions import IsAuthenticated
from rest_framework.response import Response
from .models import Book
from .serializers import BookSerializer

# określamy dostępne metody żądania dla tego endpointu
@api_view(['GET'])
def book_list(request):
    """
    Lista wszystkich obiektów modelu Book.
    """
    if request.method == 'GET':
        books = Book.objects.all()
        serializer = BookSerializer(books, many=True)
        return Response(serializer.data)


@api_view(['GET'])
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


@api_view(['PUT', 'DELETE'])
@authentication_classes([SessionAuthentication, BasicAuthentication])
@permission_classes([IsAuthenticated])
def book_update_delete(request, pk):

    """
    :param request: obiekt DRF Request
    :param pk: id obiektu Book
    :return: Response (with status and/or object/s data)
    """
    try:
        book = Book.objects.get(pk=pk)
    except Book.DoesNotExist:
        return Response(status=status.HTTP_404_NOT_FOUND)

    if request.method == 'PUT':
        serializer = BookSerializer(book, data=request.data)
        if serializer.is_valid():
            serializer.save()
            return Response(serializer.data)
        return Response(serializer.errors, status=status.HTTP_400_BAD_REQUEST)

    elif request.method == 'DELETE':
        book.delete()
        return Response(status=status.HTTP_204_NO_CONTENT)

```

Pojawienie się dekoratorów `@authentication_classes([SessionAuthentication, BasicAuthentication])` oraz `@permission_classes([IsAuthenticated])` określa odpowiednio dopuszczalne metody autentykacji oraz listę klas, które definiują wymagane uprawnienia. Bez ich spełnienia kod metody nie zostanie wykonany, a do obiektu `Response` zostanie dołączony odpowiedni kod błędu np.:
```HTTP 403 Forbidden
Allow: OPTIONS, PUT, DELETE
Content-Type: application/json
Vary: Accept

{
    "detail": "Nie podano danych uwierzytelniających."
}
```
Dodatkowo musimy również zaktualizować definicję URL-i dla zmodyfikowanych endpointów. Postać pliku `biblioteka/urls.py` po modyfikacji:


**_Listing 3_**
```python
from django.urls import path, include
from . import views

urlpatterns = [
    path('books/', views.book_list),
    path('books/<int:pk>/', views.book_detail),
    path('books/update_delete/<int:pk>/', views.book_update_delete),
]
```
Docelowo niezłym pomysłem jest budowanie oddzielnego endpointu dla każdej operacji, co zmniejszy ilość refactoringu, który będzie trzeba wykonać w przyszłości jeżeli pojawi się potrzeba dodania uprawnień dla każdego z nich z osobna.

Po przetestowaniu tych dwóch endpointów powinniśmy móc wyświetlać obiekty typu Person, ale bez uwierzytelnienia nie możemy ich modyfikować ani usuwać.

**Krok 2 - dodanie możliwości zalogowania się poprzez interfejs API dostarczony przez DRF.**

Do pliku `urls.py` w projekcie dodajemy:

**_Listing 4_**
```python
urlpatterns += [
    path('api-auth/', include('rest_framework.urls')),
]
```

Po odświeżeniu widoku DRF API pojawi się możliwość zalogowania z wykorzystaniem wcześniej utworzonych kont użytkowników. Po uwierzytelnieniu otrzymamy dostęp do wszystkich endpointów, które wymagają uprawnienia `IsAuthenticated`.


### **Autentykacja poprzez token**

**Autentyfikacja przez token w Django Rest Framework (DRF)** polega na przyznaniu użytkownikowi unikalnego tokena, który jest przesyłany w nagłówkach każdej żądanej operacji. Dzięki temu użytkownik może się uwierzytelnić bez konieczności wysyłania swoich danych logowania w każdym żądaniu.

Żądanie HTTP składa się z kilku kluczowych elementów, które pozwalają klientowi komunikować się z serwerem. Oto ich struktura:

#### 1. **Linia początkowa (Request Line)**  
Zawiera podstawowe informacje o żądaniu:
- **Metoda HTTP**: Określa typ operacji (np. `GET`, `POST`, `PUT`, `DELETE`).
- **Ścieżka URI**: Wskazuje zasób na serwerze (np. `/api/resource`).
- **Wersja protokołu HTTP**: Najczęściej `HTTP/1.1` lub `HTTP/2`.

**Przykład**:  
```
GET /api/resource HTTP/1.1
```

#### 2. **Nagłówki HTTP (Headers)**  
Metadane opisujące żądanie. Kluczowe nagłówki to:
- **Host**: Adres serwera (np. `Host: www.example.com`).
- **Authorization**: Dane uwierzytelniające (np. `Authorization: Token abc123`).
- **Content-Type**: Typ danych w treści żądania (np. `application/json`).
- **Accept**: Preferowany format odpowiedzi (np. `application/json`).
- **User-Agent**: Informacja o kliencie (np. przeglądarka lub aplikacja).

**Przykład nagłówków**:
```
Host: www.example.com
Authorization: Token abc123
Content-Type: application/json
User-Agent: curl/7.64.1
```

#### 3. **Treść żądania (Body)**  
Obecna tylko w żądaniach takich jak `POST` lub `PUT`. Zawiera dane przesyłane do serwera, np. formularze, JSON, pliki.  
**Przykład treści w formacie JSON**:
```json
{
    "name": "John",
    "email": "john@example.com"
}
```

#### 4. **Pusta linia końcowa**  
Oddziela nagłówki od treści żądania. W żądaniach bez treści jest to po prostu końcowy znak nowej linii.

#### Przykładowe pełne żądanie HTTP:
```
POST /api/resource HTTP/1.1
Host: www.example.com
Authorization: Token abc123
Content-Type: application/json
User-Agent: curl/7.64.1

{
    "name": "John",
    "email": "john@example.com"
}
```

### Jak to działa token DRF:

1. **Tworzenie tokena**: Po pomyślnym zalogowaniu użytkownik otrzymuje token, który jest generowany przez serwer.
2. **Przesyłanie tokena**: Klient przesyła token w nagłówku `Authorization: Token <token>` w każdej kolejnej żądanej operacji.
3. **Weryfikacja tokena**: Serwer sprawdza poprawność tokena i identyfikuje użytkownika.

---

### Instrukcja krok po kroku

#### 1. **Instalacja i konfiguracja Django Rest Framework**
Upewnij się, że masz zainstalowane Django i DRF:
```bash
pip install django djangorestframework
```

Jeżeli nadal nie masz to dodaj `rest_framework` do zmiennej `INSTALLED_APPS` w `settings.py`:

**_Listing 5_**
```python
INSTALLED_APPS = [
    ...
    'rest_framework',
    'rest_framework.authtoken',
]
```

Uruchom migracje, aby utworzyć odpowiednie tabele w bazie danych:
```bash
python manage.py migrate
```


#### 2. **Aktywacja autentyfikacji przez token**
W `views.py` zmień `@authentication_classes([SessionAuthentication, BasicAuthentication])` na `@authentication_classes([SessionAuthentication, TokenAuthentication])`:

**_Listing 6_**
```python
from rest_framework.authentication import TokenAuthentication

@api_view(['GET'])
@authentication_classes([SessionAuthentication, TokenAuthentication])
@permission_classes([IsAuthenticated])
def book_detail(request, pk):
```

> !!! Uwaga !!! </br>
> Jeżeli chcesz ustawić domyślną autentyfikację dla całego projektu, wtedy dodaj taki kod do pliku `settings.py` zmieniając w miejscu `DEFAULT_AUTHENTICATION_CLASSES` sposób autentyfikacji (Username & Password: `BasicAuthentication`, Token: `TokenAuthentication`), a dekoratora `@authentication_classes` używaj jedynie do widoków, które mają mieć inną autentyfikację:
>```python
>REST_FRAMEWORK = {
>    'DEFAULT_AUTHENTICATION_CLASSES': [
>        'rest_framework.authentication.TokenAuthentication',
>    ],
>    'DEFAULT_PERMISSION_CLASSES': [
>        'rest_framework.permissions.IsAuthenticated',
>    ],
>}
>```
> Dodatkowo warto wiedzieć, że przy globalnej domyślnej autentyfikacji, aby wyłączyć ją dla danego endpointa (czyli, aby również niezalogowani mogli się do niego dostać) nalezy ustawić wartość dla `permission_classes` jako `AllowAny` na przykład korzystając z dekoratora:
> ```python
> @permission_classes([AllowAny])
> def book_list(request):
> ```
> lub ze zmiennej w środku klasy jeżeli używamy widoku klasowego zamiast funkcyjnego, na przykład:
> ```python
> class PublicEndpointView(APIView):
>    permission_classes = [AllowAny]
>
>    def get(self, request):
>        return Response({"message": "This is a public endpoint, no authentication required!"})
> ```


#### 3. **Generowanie tokenów dla użytkowników**
W pliku `urls.py` dodaj widok generowania tokenów:

**_Listing 7_**
```python
from django.urls import path
from rest_framework.authtoken.views import obtain_auth_token

urlpatterns = [
    ...
    path('api-token-auth/', obtain_auth_token, name='api_token_auth'),
]
```

Po wdrożeniu możesz uzyskać token dla użytkownika, wysyłając żądanie `POST` na `/api-token-auth/` z danymi logowania na przykład używając polecenia curl:
```bash
curl -X POST -d "username=admin&password=admin" http://127.0.0.1:8000/api-token-auth/
```

bądź logując się w przeglądarce używając stworzonego endpointa z **metodą POST**.

#### 4. **Dodanie tokenów do nagłówków**
Przy każdym żądaniu API, prześlij token w nagłówku `Authorization`:
```bash
curl -H "Authorization: Token <twój_token>" http://127.0.0.1:8000/api/some-endpoint/
```

Niestety nie ma prostego sposobu, żeby dodać ręcznie taki token do pamięci przeglądarki. Można to zrobić w Postmanie, co pokażemy później.

#### 5. **Przykład API z ochroną przez token**
W `views.py` utwórz widok chroniony przez autentyfikację:

**_Listing 8_**
```python
from rest_framework.views import APIView
from rest_framework.response import Response
from rest_framework.permissions import IsAuthenticated

class ProtectedView(APIView):
    permission_classes = [IsAuthenticated]

    def get(self, request):
        return Response({"message": "Hello, authenticated user!"})
```

Dodaj trasę w `urls.py`:

**_Listing 9_**
```python
from django.urls import path
from .views import ProtectedView

urlpatterns = [
    ...
    path('protected/', ProtectedView.as_view(), name='protected'),
]
```

#### 6. **Testowanie**
1. Zaloguj się, aby uzyskać token.
2. Przetestuj chronione API, wysyłając żądanie z tokenem.


Autentykacja poprzez token wymaga dodania odpowiedniego atrybutu w nagłówku żądania. W zależności od klienta do odpytywania API, z którego korzystamy sam sposób przekazania może się różnić. 

W przypadku narzędzia `http` wiersza poleceń (Mac) może to wyglądać tak:
```bash
http http://127.0.0.1:8000/hello/ 'Authorization: Token 9054f7aa9305e012b3c2300408c3dfdf390fcddf'

# lub
http http://127.0.0.1:8000/hello/ 'Authorization: Bearer 9054f7aa9305e012b3c2300408c3dfdf390fcddf'

```

W przypadku polecenia `curl`:
```bash
curl -H "Authorization: Token 9054f7aa9305e012b3c2300408c3dfdf390fcddf" http://127.0.0.1:8000/hello/ 

# lub

curl -H "Authorization: Bearer 9054f7aa9305e012b3c2300408c3dfdf390fcddf" http://127.0.0.1:8000/hello/ 
```
 Z poziomu czystego Pythona:
 ```python
 import requests

url = 'http://127.0.0.1:8000/hello/'
# poniżej też może nastąpić konieczność zamiany słowa Token na Bearer
headers = {'Authorization': 'Token 9054f7aa9305e012b3c2300408c3dfdf390fcddf'}
r = requests.get(url, headers=headers)
```

A w programie Postman należy przejść do zakładki `Authorization`, wybrać `Bearer Token` i podać jego wartość jak na poniższym zrzucie ekranu.

![](postman_auth.png)

Postman narzuca też własną nazwę tokena co powoduje, że nagłówek żądania wygląda tak:
```python
headers = {'Authorization': 'Bearer 9054f7aa9305e012b3c2300408c3dfdf390fcddf'}
```

Jeżeli spowoduje to błąd, można to obejść na dwa sposoby:
1. Poprzez prostą własną implementację klasy `TokenAuthentication`:

**_Listing 10_**
```python
class BearerTokenAuthentication(TokenAuthentication):
    keyword = u"Bearer"
```

A następnie importujemy ją w widokach i zamieniamy nazwę `TokenAuthentication` na `BearerTokenAuthentication`. Jednak to będzie wymuszało podawanie teraz nagłówka jako `'Authorization': 'Bearer 9054f7aa9305e012b3c2300408c3dfdf390fcddf'` a nie `'Authorization': 'Token 9054f7aa9305e012b3c2300408c3dfdf390fcddf'`.

2. Poprzez wyłączenie w Postmanie autoryzacji (*Type: No Auth*):

![](postman_token_auth_1.PNG)

 a następnie ręczne dodanie nagłówka `Authorization` z wartością `Token 9054f7aa9305e012b3c2300408c3dfdf390fcddf`:

![](postman_token_auth_2.PNG)

Dodatkowo, dla widoków (endpointów), które będą uwierzytelniane poprzez token nie możemy używać jednocześnie kontroli uprawnienia `IsAuthenticated`.

## A jak to zrobić w HTML? (Zaawansowane)

#### Basic Auth:

1. Dodaj widok logowania i wylogowania w `views.py`:

```python
from django.contrib.auth import authenticate, login, logout

def user_login(request):
    if request.method == "POST":
        username = request.POST.get('username')
        password = request.POST.get('password')
        user = authenticate(request, username=username, password=password)
        if user is not None:
            login(request, user)
            return redirect('osoba-list')
        else:
            return render(request, 'biblioteka/login.html', {'error': 'Nieprawidłowe dane'})
    return render(request, 'biblioteka/login.html')

def user_logout(request):
    logout(request)
    return redirect('user-login')
```

2. Stwórz szablon `login.html`:

```html
{% extends "biblioteka/base.html" %}

{% block title %}Logowanie{% endblock %}

{% block content %}
<h2>Logowanie</h2>
{% if error %}
<p style="color:red;">{{ error }}</p>
{% endif %}
<form method="post">
    {% csrf_token %}
    <p>Username: <input type="text" name="username"></p>
    <p>Password: <input type="password" name="password"></p>
    <p><input type="submit" value="Zaloguj"></p>
</form>
{% endblock %}
```

3. Zabezpiecz widok dekoratorem `@login_required`. Dekorator ten przyjmuje jako parametr nazwany `login_url` nazwę erefencyjną widoku do logowania.

```python
from django.contrib.auth.decorators import login_required

@login_required(login_url='user-login')
def osoba_list_html(request):
    # reszta kodu funkcji :)
```

4. Dodaj przyciski do logowania i wylogowywania do `base.html`:

W Django w szablonach możemy sprawdzić, czy użytkownik jest zalogowany za pomocą `request.user.is_authenticated`. Możemy to wykorzystać do dodania przycisku wylogowywania.

Zmodyfikowany plik `base.html` wyglądąłby zatem następująco:

```html
<!DOCTYPE html>
<html>
    <head>
        <title>{% block title %}{% endblock %}</title>
    </head>
    <body>
        <div id="header">
            Pierwsza aplikacja Django
            {% if request.user.is_authenticated %}
                Witaj, {{ request.user.username }}!
                <a href="{% url 'user-logout' %}">Wyloguj</a>
            {% else %}
                <a href="{% url 'user-login' %}">Zaloguj</a>
            {% endif %}
        </div>
        <hr/>
        <div id="content">
            {% block content %}{% endblock %}
        </div>
        <div id="sidebar">
            <p>Panel boczny</p>
        </div>
    </body>
</html>

```

5. Dodaj nowe endpointy do `urls.py` w folderze aplikacji:

```python
    path('login/', views.user_login, name='user-login'),
    path('logout/', views.user_logout, name='user-logout'),
    path('osoby/', views.osoba_list_html, name='osoba-list-html'),
```

#### TokenAuthentication:

Niestety z tokenem dla HTML jest nieco ciężej...

1. W `views.py` można zrobić prosty endpoint logowania i wylogowania tokenowego:
```python
from rest_framework.authtoken.models import Token

def drf_token_login(request):
    if request.method == "POST":
        username = request.POST.get('username')
        password = request.POST.get('password')
        user = authenticate(username=username, password=password)
        if user:
            token, created = Token.objects.get_or_create(user=user)
            # zapisujemy token w sesji
            request.session['token'] = token.key
            request.session['user_id'] = user.id
            return redirect('osoba-list')
        else:
            return render(request, 'biblioteka/login.html', {'error': 'Nieprawidłowe dane'})
    return render(request, 'biblioteka/login.html')

def drf_token_logout(request):
    request.session.flush()
    return redirect('drf-token-login')
```

2. Teraz możemy stworzyć swój własny dekorator...

W tradycyjnych widokach HTML nie ma wbudowanego dekoratora session+token. Dlatego musimy utworzyć swój własny i umieścić go np. w `views.py`:

```python
from functools import wraps

def drf_token_required(view_func):
    @wraps(view_func)
    def _wrapped_view(request, *args, **kwargs):
        token_key = request.session.get('token')
        if not token_key:
            return redirect('drf-token-login')
        try:
            Token.objects.get(key=token_key)
        except Token.DoesNotExist:
            return redirect('drf-token-login')
        return view_func(request, *args, **kwargs)
    return _wrapped_view

```

3. Zabezpieczenie widoku listy osób
```python
@drf_token_required
def osoba_list_html(request):
```

4. Modyfikacja fragmentu `base.html` do logowania:
```html
            {% if request.session.token %}
                <a href="{% url 'drf-token-logout' %}">Wyloguj</a>
            {% else %}
                <a href="{% url 'drf-token-login' %}">Zaloguj</a>
            {% endif %}
```

5. Dodajemy do `urls.py` w naszej aplikacji logowanie tokenem:
```python
    path('token/login/', views.drf_token_login, name='drf-token-login'),
    path('token/logout/', views.drf_token_logout, name='drf-token-logout'),
```


## **Zadania**

**Zadanie 1** 

W aplikacji rozwijanej w trakcie zajęć odtwórz opisany powyżej sposób uwierzytelniania (listing 4 i uwierzytelnianie przez wcześniej utworzone konto użytkownika Django np. superusera) i go przetestuj.

**Zadanie 2** 

 Do klasy `Osoba` dodaj pole `wlasciciel`, które będzie odwoływało się do modelu `User` wbudowanego w Django Auth. Ogranicz możliwość wyświetlania rekordów tylko ich właścicielom (na widoku wyświetlającym wszystkie obiekty typu `Osoba`). Przetestuj z użyciem dwóch różnych kont użytkowników. W tutorialu (https://www.django-rest-framework.org/tutorial/4-authentication-and-permissions/#tutorial-4-authentication-permissions) przedstawiony jest taki scenariusz z przykładami. Nie wystawiaj jednak API dla modelu `User` jeżeli nie jest to konieczne.

> Wskazówka: Jeżeli dostęp do endpointu, który zapisuje nowe obiekty `Osoba` został zadeklarowany przez widok funkcyjny (a nie poprzez dziedziczenie z klasy `APIView`) to dodanie obiektu aktualnie zalogowanego usera do danych z żądania POST można zrealizować tak:
```python
...
    if request.method == 'POST':
        serializer = OsobaSerializer(data=request.data)
        if serializer.is_valid():
            serializer.save(wlasciciel=request.user)
            return Response(serializer.data)
        return Response(serializer.errors, status=status.HTTP_400_BAD_REQUEST)
```
**Zadanie 3** 

 Korzystając z dokumentacji zawartej w https://www.django-rest-framework.org/api-guide/authentication/#tokenauthentication dodaj obsługę tokenów w aplikacji. Dodaj również tokeny dla istniejących userów.

**Zadanie 4** 

Rozbij endpoint dla Osoba na oddzielne punkty dla żądania typu PUT oraz DELETE i dla tego drugiego ustaw autentykację poprzez token. Przetestuj działanie za pomocą narzędzia curl lub Postman.

**Zadanie 5** 

 Dodaj lub zmodyfikuj istniejący endpoint tak aby możliwe było wyświetlenie wszystkich obiektów typu `Osoba` przypisanych do danego stanowiska. Niech url dla niego będzie postaci `.\stanowisko\<id>\members\`. Niech dostęp będzie tylko do odczytu i tylko poprzez autoryzację tokenem.
