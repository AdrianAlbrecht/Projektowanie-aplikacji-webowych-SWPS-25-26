# Projektowanie aplikacji webowych, semestr 2025Z

## 1. Cel i zakres przedmiotu

Celem przedmiotu jest przybliżenie osobie studenckiej zagadnień związanych z projektowaniem aplikacji WWW z wykorzystaniem języka Python oraz frameworka Django. W trakcie zajęć główny nacisk zostanie położony na backend (czyli część aplikacji związana z implementacją funkcjonalności po stronie serwera), ale zajmiemy się też budową prostych widoków, czyli frontendem.

W trakcie zajęć osoba studencka pozna m.in. zagadnienia takie jak:
* konfiguracja bazy danych na potrzeby aplikacji
* obsługa narzędzia Git
* tworzenie i zarządzanie modelami we frameworku Django
* migracja bazy i rozwiązywanie problemów z migracjami
* implementacja logiki biznesowej (wymagań klienta) w projekcie
* tworzenie API REST-owego na potrzeby różnych technologii frontendowych
* widoki i szablony Django z wykorzystaniem modeli i formularzy
* obsługa i implementacja autentykacji i uwierzytelniania z użyciem Django
* poznanie narzędzi zarządzania projektem Django oraz panelu administratora i jego możliwości


## 2. Oprogramowanie

W trakcie zajęć do pracy niezbędne będzie posiadanie:
* zainstalowanego interpretera **Pythona** w wersji 3.9 lub nowszej
* **framework Django** w wersji 4.2 lub nowszej
* narzędzie IDE, preferowane **Visual Studio Code**, ale zachęcam do używania również **PyCharm Professional** (licencja studencka) lub wersja Community. Może to być również inne oprogramowanie ze wsparciem dla języka Python. Na zajęciach prowadzący będzie używał **Visual Studio Code**.
* narzędzie Git do zarządzania kodem projektu
* możliwe, że w zależności od konfiguracji projektu niezbędne będzie zainstalowanie i konfiguracja odpowiedniego serwera bazy danych

## 3. Warunki zaliczenia przedmiotu.

- Efektem finalnym pracy na zajęciach będzie **projekt API** stworzony w Django Rest Framework lub po uzgodnieniu w innym frameworku (o ile osoba studencka dany framework zna),
- osoby studenckie mogą dobrać się w pary lub pracować samodzielnie (w wyjątkowych przypadkach dozwolona jest praca w 3-osobowej grupie),
- Projekt osoby studenckiej będzie przechowywany w **publicznym repozytorium GitHub**, do którego prowadzący będzie miał wgląd przez cały okres trwania zajęć (najpóźniej przesłany link do 3 zajęć z przedmiotu),
   - W przypadku pracy grupowej, obie osoby muszą być dodane jako collaborator do repozytorium,
- Oceniane będą:
   - Systematyczność commitów (oraz ich [sens](https://medium.com/@steveamaza/how-to-write-a-proper-git-commit-message-e028865e5791)!!!!),
   - Jakość kodu programistycznego (brak kodu spaghetti),
   - Implementacja zagadnień przedstawionych na zajęciach,
   - Poziom skomplikowania projektu według wymagań (np. ilość modeli, działanie endpointów, itp.)
- Proponowany temat API (co te API będzie robić) jest dowolny i będzie przedstawiony prowadzącemu na drugich zajęciach.
- Prowadzący rezerwuje możliwość przeprowadzenia kolokwium w przypadku braku pracy na zajęciach przez grupę osób studenckich.

## 4. Wymagania projektu.

Zakres projektu zaliczeniowego:
1.	**Modele**:
* 4-5 adekwatnych do projektu modeli
2.	**Uwierzytelnianie (preferowany token) i autoryzacja.**
* Co najmniej 2 konta z różnym poziomem dostępu do endpointów (np. admin, user)
3.	**Endpointy:**
* Rejestracja użytkownika aplikacji
* CRUD (Create, Read, Update, Delete)
* Minimum 2 endpointy, które wyjdą poza schemat CRUD, np. zestawienie miesięczne zamówień, lista wypożyczeń dla użytkownika, lista towarów zaczynających się od itp.
4. **Inne:**
* *"Idiotoodporność"* - czyli żeby nie móc wpisać tekstu w miejsce daty oraz aby daty nie były z przeszłości, itd.
* Walidacja danych
* Sens i działanie (szeroko pojęte)

## 5. Rozpiska
1. Przygotowania środowiska pracy oraz utworzenia przykładowego projektu Django i omówienia jego struktury
2. Omówienie i przydzielenie projektów. Zapoznanie z narzędziem wersjonowania projektu - GIT.
3. Schemat bazy danych.
4. Modele Django, migracja danych, Django admin panel. (2 zajęcia)
5. Widoki i szablony Django z wykorzystaniem modeli i formularzy.
6. REST API - serializacja, Django Rest Framework (2 zajęcia)
7. Autentykacja
8. Autoryzacja
9. Testowanie aplikacji (opcjonalnie)
10. Prezentacja i ocena projektów.

## 6. Obecność i Oceny
Na zajęciach sprawdzana będzie obecność. Dopuszczone są 3 nieobecności nieusprawiedliwione. 

Z zajęć uzyskać można maksymalnie **50 punktów**, z czego:
* Systematyczność commitów (oraz ich sens) - max 5 pkt.
* Implementacja zagadnień przedstawionych na zajęciach - max. 5 pkt.
* Modele adekwatne do projektu - max. 5 pkt.
* Uprawnienia i konta - max. 5 pkt.
* Rejestracja i logowanie - max. 5 pkt.
* CRUD - max. 10 pkt.
* 2 endpointy poza CRUD - max. 5 pkt.
* Walidacja -  max. 5 pkt.
* *"Idiotoodporność"* - max. 5 pkt.

Dla ukazania "ocen" (jeżeli byłoby to potrzebne z jakiś względów) suma punktów do ocen ukazuje się następująco:
- 91%+ ===> 45,5+ pkt. ===> 5
- 81 - 90% ===> 40,5 - 45 pkt ===> 4,5
- 71 - 80% ===> 35,5 - 40 pkt ===> 4
- 61 - 70% ===> 30,5 - 35 pkt ===> 3,5
- 51 - 60% ===> 25,5 - 30 pkt ===> 3
- 0 - 50% ===> 0 - 25 pkt ===> 2

Innymi słowy trzeba uzyskać minimum 51% punktów, aby zaliczyć przedmiot.

> # WAŻNA INFORMACJA - PRZYPOMNIENIE!
> **Projekt zaliczeniowy, to zaliczenie przedmiotu!**
>
>Zgodnie z [REGULAMINEM STUDIÓW W SWPS UNIWERSYTECIE HUMANISTYCZNOSPOŁECZNYM](https://bip.swps.pl/attachments/1020/download) §6 pkt 2 ppkt 4) i ppkt 6) student ma obowiązek do _"**poszanowania praw własności intelektualnej** i dóbr osobistych osób trzecich, w szczególności majątkowych i osobistych praw autorskich oraz przestrzegania uczelnianych przepisów dotyczących praw własności intelektualnej"_ oraz  _"rzetelnego, terminowego i **samodzielnego** wykonywania zadań nałożonych przez osoby prowadzące zajęcia, a wynikających z realizowanego programu studiów oraz decyzji organów i osób działających w imieniu Uczelni w zakresie organizacji studiów"_, co jednoznacznie odnosi się do samodzielności wykonania m.in. prac zaliczeniowych.
>
> Przypominam również, że zgodnie z §8  _"Student podlega odpowiedzialności dyscyplinarnej za naruszenie przepisów obowiązujących w Uczelni oraz za czyn uchybiający godności studenta na zasadach określonych w Ustawie."_, co może skutkować nawet skreśleniem z listy studentów.
>
> Informuję, że w związku z tym **każda praca zaliczeniowa, która będzie osądzona o pracę niesamodzielną i zostanie to udowodnione i zweryfikowane przez prowadzącego oceniona będzie na 0 pkt**.


## Kontakt do prowadzących
- Krzysztof Ropiak ([kropiak@swps.edu.pl](mailto:kropiak@swps.edu.pl))
- Adrian Albrecht ([aalbrecht@swps.edu.pl](mailto:aalbrecht@swps.edu.pl))
