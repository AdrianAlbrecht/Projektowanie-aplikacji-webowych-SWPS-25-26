# Projektowanie Aplikacji Webowych, semestr 2025Z

## Lab 10 - Uprawnienia w aplikacji Django oraz w Django Rest Framework.

> **Materiały uzupełniające:**
> * Ciekawy artykuł o uprawnieniach dla Django: https://realpython.com/manage-users-in-django-admin/ (UWAGA: linki odnoszą do dokumentacji dla wersi 2.x Django)
> * Django i uprawnienia: https://docs.djangoproject.com/pl/5.2/topics/auth/default/#permissions-and-authorization


## 1. Uprawnienia, a panel administracyjny Django.

Django posiada wbudowany moduł `auth`, dzięki któremu dostarczany jest mechanizm uwierzytelniania i autoryzacji dla użytkowników aplikacji. Jeżeli aplikacje `django.contrib.admin` oraz `django.contrib.auth`  są zainstalowane to w ramach panelu administracyjnego możliwe jest tworzenie użytkowników, grup i przypisywanie im uprawnień.

Ogólna zasada zarządzania uprawnieniami jest taka, iż powinniśmy uprawnienia przypisywać do grup, a nie bezpośrednio do użytkowników. To pozwala na tworzenie różnych poziomów uprawnień (możemy przypisać użytkownika do wielu grup) i zdecydowanie łatwiejszego nimi zarządzania (pojawienie się nowego użytkownika danego działu nie wymaga kopiowania pojedynczych uprawnień tylko przypisania go do odpowiedniej grupy).

Domyślne uprawnienia dla modeli w panelu administracyjnym, będą kontrolowane przez ten panel dla użytkowników w zespole (atrybut użytkownika), którym ten atrybut umożliwia zalogowanie się w panelu. Pozostałe, stworzone przez nas uprawnienia można przypisywać poprzez panel, ale nie zadziałają z automatu i ich reguły muszą być określone na poziomie np. modeli administracyjnych (`ModelAdmin`). Właściwe wykorzystanie tych właściwości i wykorzystanie własnej implementacji modelu użytkownika (przesłonięcie wbudowanego modelu `User`) pozwala zbudować całkiem funkcjonalną aplikację opartą o Django admin.

## 2. Uprawnienia a modele Django.

Django posiada wbudowany mechanizm podstawowych uprawnień dla każdego modelu, który w aplikacji zostanie utworzony. Dla każdego modelu zostaną utworzone cztery prawa:

* `add` - pozwala na stworzenie nowej instancji modelu,
* `delete` - pozwala na usunięcie instancji modelu,
* `change` - pozwala na aktualizację instancji modelu,
* `view` - pozwala na wyświetlenie instancji obiektu.

Aby całość funkcjonowała poprawnie w ekosystemie projektu nazwy uprawnień nadawane są wg. konwencji:
`<nazwa_aplikacji>.<akcja>_<nazwa_modelu>` np. `biblioteka.change_osoba`.

> **Każdy superuser posiada wszystkie prawa, chociaż bliższe prawdy jest stwierdzenie, że tak na prawdę te uprawnienia nie są dla niego sprawdzane w momencie użycia metody `has_perm(uprawnienie)`, które dla tekiego uzytkownika zawsze zwraca wartość `True`.**


Samo istnienie uprawnień nie powoduje, że w każdym miejscu aplikacji działa mechanizm ich weryfikacji, jeżeli na danym modelu odbywa się jedna z operacji CRUD. Jedynym miejscem, w którym się to odbywa jest panel administracyjny.

W każdym innym miejscu musimy zaimplementować metody sprawdzające posiadanie przez użytkownika wykonującego daną akcję posiadanie przez niego stosownych uprawnień.

**_Listing 1_**  
```python
from django.core.exceptions import PermissionDenied

@login_required(login_url='user-login')
def osoba_view(request, pk):
    if not request.user.has_perm('biblioteka.view_osoba'):
        raise PermissionDenied()
    try:
        osoba = Osoba.objects.get(pk=pk)
        return HttpResponse(f"Ten użytkownik nazywa się {osoba.imie} {osoba.nazwisko}")
    except Osoba.DoesNotExist:
        return HttpResponse(f"W bazie nie ma użytkownika o id={pk}.")
```

```python
path("osoby/<int:pk>/permisje/", views.osoba_view),
```

Można wykorzystać również dekoratory na wzór:

**_Listing 2_**

```python
from django.contrib.auth.decorators import permission_required

@login_required(login_url='user-login')
@permission_required('biblioteka.view_osoba', raise_exception=True)
def osoba_view_decorator(request, pk):
    try:
        osoba = Osoba.objects.get(pk=pk)
        return HttpResponse(f"Ten użytkownik nazywa się {osoba.imie} {osoba.nazwisko}")
    except Osoba.DoesNotExist:
        return HttpResponse(f"W bazie nie ma użytkownika o id={pk}.")
```

```python
path("osoby/<int:pk>/permisje_decorator/", views.osoba_view_decorator),
```

Oprócz wbudowanych uprawnień dla modeli możliwe jest również zdefiniowanie dodatkowych uprawnień w klasie wewnętrznej `Meta` każdego modelu.

**_Listing 3_**

```python
class Osoba(models.Model):
    ...
    class Meta:
        permissions = [
            ("change_osoba_owner", "Pozwala przypisać inną osobę do obiektu Osoba."),
            ("change_assign_to_stanowisko", "Pozwala przypisać osobę do innego stanowiska."),
        ]
```
Aby takie uprawnienia pojawiły się w panelu administracyjnym i było możliwe ich wykorzystanie w projekcie należy wykonać proces migracji, gdyż funkcja tworząca uprawnienia jest powiązana z sygnałem `post_migrate`.
Implementacja logiki tych uprawnień spoczywa w całości na programiście.

Możliwe jest również weryfikowanie uprawnień na poziomie szablonów widoków Django (jeżeli to jest nasza docelowa technologia wizualizacji dla projektu) odwołując się do zmiennej `perms` w szablonie.

**_Listing 4_** (fragment szablonu `osoba\detail.html`)
```html
#...
{% if perms.biblioteka.delete_osoba %}
    <form method="post">
        {% csrf_token %}
        <button type="submit" onclick="return confirm('Czy na pewno chcesz usunąć tę osobę?');">
            Usuń osobę
        </button>
    </form>
{% endif %}
#...
```

## 3. Uprawnienia a widoki HTML

`permission_required` zadziała również w HTML, ale musimy go użyć poprawnie i najlepiej razem z `login_required` albo zamiast niego.

**_Listing 5.1_**
```python
from django.contrib.auth.decorators import permission_required

@permission_required('biblioteka.view_osoba', login_url='user-login')
def osoba_list_html(request):
```

lub

**_Listing 5.2_**
```python
@login_required(login_url='user-login')
@permission_required('biblioteka.view_osoba', raise_exception=True)
def osoba_list_html(request):
```

Tutaj `raise_exception=True` spowoduje wywołanie statusu HTML 403 Forbidden, zamiast przekierowania.

---

Trochę ciężej się robi, gdy chcemy użyć tokenu. `permission_required` sprawdza uprawnienia na `request.user`, a nasz dekorator `drf_token_required` niestety nie aktualizuje tego pola naszego zapytania. Powinniśmy zatem zmienić delikatnie nasz dekorator:

**_Listing 6_**
```python
def drf_token_required(view_func):
    @wraps(view_func)
    def _wrapped_view(request, *args, **kwargs):
        token_key = request.session.get('token')
        if not token_key:
            return redirect('drf-token-login')
        try:
            # tutaj przypiszmy ten Token do zmiennej `token`
            token = Token.objects.get(key=token_key)
        except Token.DoesNotExist:
            return redirect('drf-token-login')
        # a tutaj zaktualizujmy nasze pole `user` w zapytaniu
        request.user = token.user
        return view_func(request, *args, **kwargs)
    return _wrapped_view
```

Teraz możemy już używać naszego dekoratora wraz z permisjami:
> !!! UWAGA !!! Kolejność dekoratorów ma znaczenie


**_Listing 7_**
```python
@drf_token_required
@permission_required('biblioteka.view_osoba', raise_exception=True)
def osoba_list_html(request):
```

## 4.Uprawnienia a Django Rest Framework

Django Rest Framework dostarcza kilka wbudowanych uprawnień, z których część została przedstawiona na poprzednich zajęciach (np. `IsAuthenticated`).

Pozostałe zostały opisane tutaj: https://www.django-rest-framework.org/api-guide/permissions/#api-reference

### 1. **`AllowAny`**
Daje dostęp wszystkim użytkownikom, niezależnie od tego, czy są uwierzytelnieni.

**Przykład:**
```python
from rest_framework.permissions import AllowAny
from rest_framework.views import APIView
from rest_framework.response import Response

class PublicView(APIView):
    permission_classes = [AllowAny]

    def get(self, request):
        return Response({"message": "Accessible by anyone!"})
```

### 2. **`IsAuthenticated`**
Pozwala na dostęp tylko użytkownikom zalogowanym.

**Przykład:**
```python
from rest_framework.permissions import IsAuthenticated
from rest_framework.views import APIView
from rest_framework.response import Response

class PrivateView(APIView):
    permission_classes = [IsAuthenticated]

    def get(self, request):
        return Response({"message": f"Hello, {request.user.username}!"})
```

### 3. **`IsAdminUser`**
Pozwala na dostęp tylko użytkownikom, którzy mają flagę `is_staff=True`.

**Przykład:**
```python
from rest_framework.permissions import IsAdminUser
from rest_framework.views import APIView
from rest_framework.response import Response

class AdminOnlyView(APIView):
    permission_classes = [IsAdminUser]

    def get(self, request):
        return Response({"message": "Accessible only by admin users."})
```

### 4. **`IsAuthenticatedOrReadOnly`**
Pozwala na pełny dostęp użytkownikom uwierzytelnionym, ale użytkownicy niezalogowani mają tylko dostęp do odczytu (np. metody `GET`, `HEAD`, `OPTIONS`).

**Przykład:**
```python
from rest_framework.permissions import IsAuthenticatedOrReadOnly
from rest_framework.views import APIView
from rest_framework.response import Response

class AuthenticatedOrReadOnlyView(APIView):
    permission_classes = [IsAuthenticatedOrReadOnly]

    def get(self, request):
        return Response({"message": "Anyone can read this."})

    def post(self, request):
        return Response({"message": f"Hello, {request.user.username}, your data has been saved."})
```

### 5. **`DjangoModelPermissions`**
Dostęp oparty na uprawnieniach przypisanych w modelach Django. Użytkownik musi być uwierzytelniony i mieć przypisane odpowiednie uprawnienia (np. `add`, `change`, `delete`, `view`).

**Przykład:**
```python
from rest_framework.permissions import DjangoModelPermissions
from rest_framework.viewsets import ModelViewSet
from myapp.models import MyModel
from myapp.serializers import MyModelSerializer

class MyModelViewSet(ModelViewSet):
    queryset = MyModel.objects.all()
    serializer_class = MyModelSerializer
    permission_classes = [DjangoModelPermissions]
```

### 6. **`DjangoModelPermissionsOrAnonReadOnly`**
Rozszerzenie `DjangoModelPermissions`. Użytkownicy anonimowi mają dostęp tylko do odczytu.

**Przykład:**
```python
from rest_framework.permissions import DjangoModelPermissionsOrAnonReadOnly
from rest_framework.viewsets import ModelViewSet
from myapp.models import MyModel
from myapp.serializers import MyModelSerializer

class MyModelViewSet(ModelViewSet):
    queryset = MyModel.objects.all()
    serializer_class = MyModelSerializer
    permission_classes = [DjangoModelPermissionsOrAnonReadOnly]
```

### 7. **`DjangoObjectPermissions`**
Kontroluje dostęp do poszczególnych obiektów za pomocą uprawnień przypisanych do nich.

**Przykład:**
```python
from rest_framework.permissions import DjangoObjectPermissions
from rest_framework.viewsets import ModelViewSet
from myapp.models import MyModel
from myapp.serializers import MyModelSerializer

class MyModelViewSet(ModelViewSet):
    queryset = MyModel.objects.all()
    serializer_class = MyModelSerializer
    permission_classes = [DjangoObjectPermissions]
```

> **Uwaga:** Należy włączyć `django-guardian` lub podobny system do obsługi uprawnień obiektowych.

### 8. **Custom Permissions (Własne uprawnienia)**
Możemy definiować własne klasy uprawnień, implementując metodę `has_permission` i/lub `has_object_permission`.

**Przykład:**
```python
from rest_framework.permissions import BasePermission
from rest_framework.views import APIView
from rest_framework.response import Response

class IsOwner(BasePermission):
    def has_object_permission(self, request, view, obj):
        return obj.owner == request.user

class OwnerOnlyView(APIView):
    permission_classes = [IsOwner]

    def get(self, request, obj):
        return Response({"message": "You are the owner of this object."})
```


>**Wykorzystanie istniejących uprawnień modeli w DRF.**
>
>Dokumentacja: https://www.django-rest-framework.org/api-guide/permissions/#djangomodelpermissions
>
><del>Z racji tego, że aktualnie nie ma oficjalnego rozwiązania pozwalającego na wykorzystanie `DjangoModelPermissions` w przypadku wykorzystania widoków funkcyjnych (to te, gdzie używamy dekoratora `@api_view`) należy niezbędną logikę widoków dla DRF przepisać na class based views.</del>
>
> **UPDATE:** Można jednak obejść problem wykorzystania `DjangoModelPermissions` w widokach funkcyjnych ustawiając ten mechanizm jako główny w pliku `settings.py`, a następnie w widokach przesłaniając go innymi wartościami dekoratora `@permission_classes`
>
>Wpis w `settings.py`:
>
>**UWAGA! - tu użyta klasa rozszerzająca DjangoModelPermissions - patrz listing 9).**
>```python
>REST_FRAMEWORK = {
>    'DEFAULT_PERMISSION_CLASSES': (
>        'biblioteka.permissions.CustomDjangoModelPermissions',
>    )
>}
>```
>```python
># i teraz widok funkcyjny może wyglądać tak
>@api_view(['GET'])
>@permission_classes([IsAuthenticated, CustomDjangoModelPermissions])
>@authentication_classes([BearerTokenAuthentication])
>def book_detail(request, pk):
>
>    """
>    :param request: obiekt DRF Request
>    :param pk: id obiektu Book
>    :return: Response (with status and/or object/s data)
>    """
>
>    if request.method == 'GET':
>        try:
>            book = Book.objects.get(pk=pk)
>        except Book.DoesNotExist:
>            return Response(status=status.HTTP_404_NOT_FOUND)
>
>        serializer = BookSerializer(book)
>        return Response(serializer.data)
>```
>
>Przykład dla modelu Author poniżej.
>
>**_Listing 8_** (Nie dodajemy tego do kodu)
>
>```python
>class Author(APIView):
>    authentication_classes = [BearerTokenAuthentication]
>    permission_classes = [IsAuthenticated, CustomDjangoModelPermissions]
>
>    # dodanie tej metody lub pola klasy o nazwie queryset jest niezbędne
>    # aby DjangoModelPermissions działało poprawnie (stosowny błąd w oknie
>    # konsoli nam o tym przypomni)
>    def get_queryset(self):
>        return Author.objects.all()
>
>    def get_object(self, pk):
>        try:
>            return Author.objects.get(pk=pk)
>        except Author.DoesNotExist:
>            raise Http404
>
>    def get(self, request, pk, format=None):
>        author = self.get_object(pk)
>        serializer = AuthorSerializer(author)
>        return Response(serializer.data)
>
>    def put(self, request, pk, format=None):
>        author = self.get_object(pk)
>        serializer = AuthorSerializer(author, data=request.data)
>        if serializer.is_valid():
>            serializer.save()
>            return Response(serializer.data)
>        return Response(serializer.errors, status=status.HTTP_400_BAD_REQUEST)
>
>    def delete(self, request, pk, format=None):
>        author = self.get_object(pk)
>        author.delete()
>        return Response(status=status.HTTP_204_NO_CONTENT)
>```
>
>W dokumentacji znajdziemy informacje o mapowaniu żądań na uprawnienia:
>* żądania `POST` wymagają posiadania prawa `add` na modelu,
>* żądania `PUT` i `PATCH` wymagają posiadania prawa `change` na modelu,
>* żądania `DELETE` wymagają posiadania prawa `delete` na modelu.
>
>A gdzie żądanie `GET`? Otóż chociaż mogłoby się wydawać logicznym, że powinno być mapowanie na uprawnienie `view` dla modelu, to tak nie jest. Można jednak rozszerzyć bazową klasę `DjangoModelPermissions` i dodać tę funkcjonalność.
>
>**_Listing 9_** 
>
>Kod został umieszczony w **NOWO STWORZONYM** pliku `permissions.py` w folderze aplikacji (nie projektu).
>```python
>import copy
>
>from rest_framework import permissions
>
>class CustomDjangoModelPermissions(permissions.DjangoModelPermissions):
>
>    def __init__(self):
>        self.perms_map = copy.deepcopy(self.perms_map)
>        self.perms_map['GET'] = ['%(app_label)s.view_%(model_name)s']
>```
>
>### A jak zdefiniować swoje customowe uprawnienie w DRF wykorzystująć już uprawnienia dodane w modelu?
>
>Przykład:
>**_Listing 10_** 
>```python
>from rest_framework.permissions import BasePermission
>
>class CanViewOsoba(BasePermission):
>    def has_permission(self, request, view):
>        return request.user.has_perm('biblioteka.view_osoba')
>```
>
>I teraz możemy dodać taką permisję do naszego widoku FBV (Function Based View):
>
>```python
>@permission_classes([IsAuthenticated, CanViewOsoba])
>```
>
>lub CBV (Class Based View):
>
>```python
>permission_classes = [IsAuthenticated, CanViewOsoba]
>```

## **Zadania**

**Zadanie 1**  
Dodaj nową grupę w panelu administracyjnym. Dodaj do tej grupy jedno wybrane uprawnienie domyślne dla modelu, który został dodany do zarządzania w panelu administracyjnym. Stwórz nowego użytkownika, który będzie "w zespole", ale nie będzie superużytkownikiem i przypisz go do tej grupy. Zaloguj się na konto stworzonego użytkownika i sprawdź czy kontrola tego uprawnienia działa poprawnie.

**Zadanie 2**  
Korzystając z przykładów z listingów 1 oraz 2 dodaj prosty widok, w logice którego sprawdź czy user posiada uprawnienie `view_osoba` i wyświetlaj odpowiednią treść. **(patrz listing 1)**

**Zadanie 3**  
Do modelu `Osoba` dodaj własne uprawnienie o nazwie `can_view_other_owners`, które dodaj do logiki z zadania 2 i jeżeli jest ono przypisane to pozwalaj wyświetlać obiekty modelu `Osoba`, których zalogowany użytkownik nie jest właścicielem. W przeciwnym wypadku nie daj takiej możliwości.

**Zadanie 4** (Cięższe zadanie, opcjonalne, dla chętnych)  
Przetestuj działanie klasy `DjangoModelPermissions` (lub `CustomDjangoModelPermissions`) z DRF z różnymi prawami dostępu (GET, PUT, POST, DELETE). Pamiętaj, że użytkownikowi, który nie jest superuserem należy przypisać stosowne prawa do modelu (poprzez przypisanie ich do grupy).
