# Privacy Gateway — ogólny zarys projektu

## 1. Wizja projektu

Celem projektu Privacy Gateway jest stworzenie platformy internetowej, która pozwoli użytkownikom korzystać z wybranych zasobów internetu za pośrednictwem serwera pośredniczącego, przy jednoczesnym ograniczeniu ilości udostępnianych danych osobowych.

Platforma ma łączyć funkcje prywatnego dostępu do stron internetowych z narzędziami ułatwiającymi zarządzanie danymi kont, pocztą elektroniczną oraz weryfikacją telefoniczną.

Najważniejszymi założeniami są prywatność, bezpieczeństwo, prostota obsługi i możliwość stopniowego rozszerzania funkcjonalności.

## 2. Podstawowy scenariusz użycia

Docelowy scenariusz działania platformy:

1. Użytkownik otwiera stronę Privacy Gateway.
2. Wprowadza adres strony internetowej, do której chce uzyskać dostęp.
3. Platforma weryfikuje adres i sprawdza, czy obsługa danej strony jest dozwolona.
4. Żądanie jest przekazywane przez serwer pośredniczący, zgodnie z możliwościami obsługiwanej architektury.
5. Użytkownik korzysta z dostępnych funkcji strony.
6. Jeżeli użytkownik chce utworzyć konto w zewnętrznym serwisie, może skorzystać z narzędzi do generowania danych oraz obsługiwanej integracji pocztowej.
7. Jeżeli serwis wymaga CAPTCHA, użytkownik rozwiązuje ją samodzielnie.
8. Jeżeli wymagana jest weryfikacja telefoniczna, użytkownik może w przyszłości skorzystać z obsługi numerów udostępnianych przez platformę.
9. Po zakończeniu sesji dane tymczasowe są usuwane zgodnie z przyjętą polityką retencji.

Nie każda strona będzie możliwa do obsłużenia przez zwykłe proxy. Niektóre witryny wymagają bezpośredniej obsługi JavaScriptu, specyficznych ciasteczek, WebSocketów lub dodatkowych mechanizmów bezpieczeństwa. Zgodność z nimi będzie weryfikowana w trakcie rozwoju projektu.

## 3. Główne moduły systemu

### 3.1. Interfejs użytkownika

Odpowiada za komunikację użytkownika z platformą.

Planowane elementy:
- strona główna z polem do wpisania adresu URL;
- panel aktywnej sesji;
- zakładka zarządzania wygenerowanymi danymi;
- zakładka poczty elektronicznej;
- przyszła zakładka numerów telefonicznych;
- ustawienia prywatności i zarządzania danymi.

Interfejs powinien być prosty, responsywny i zrozumiały również dla osób bez wiedzy technicznej.

### 3.2. Moduł proxy

Odpowiada za pośredniczenie w komunikacji pomiędzy użytkownikiem a obsługiwanymi serwisami.

Planowane zadania:
- obsługa żądań w ramach dozwolonych protokołów;
- kontrola adresów docelowych;
- zarządzanie sesjami;
- ograniczanie czasu życia danych tymczasowych;
- kontrola dostępu do zasobów i limitów ruchu;
- zabezpieczenie przed wykorzystaniem serwera do ataków lub nieautoryzowanego dostępu do sieci wewnętrznych.

Ważne: klasyczne proxy HTTP nie jest tym samym co pełna zdalna przeglądarka. W MVP należy wybrać i przetestować konkretny model działania, zamiast zakładać zgodność ze wszystkimi witrynami.

### 3.3. Moduł generowania danych

Moduł umożliwi tworzenie losowych danych na potrzeby obsługiwanych procesów.

Planowane funkcje:
- generowanie unikalnych identyfikatorów;
- generowanie silnych haseł;
- możliwość użycia własnych danych;
- przypisywanie danych do sesji lub konta platformy;
- bezpieczne usuwanie danych tymczasowych.

Dane dostępowe nie powinny być zapisywane w zwykłych logach ani przechowywane w postaci jawnej bez uzasadnionej potrzeby.

### 3.4. Moduł poczty elektronicznej

Moduł będzie obsługiwać dwa potencjalne modele:

**A. Tymczasowa skrzynka e-mail**
- utworzenie lub przydzielenie adresu;
- odbieranie wiadomości;
- wyświetlanie kodów weryfikacyjnych;
- automatyczne wygaszanie skrzynki zgodnie z polityką usługi.

**B. Oddzielna skrzynka użytkownika**
- integracja z dostawcą umożliwiającym tworzenie lub przydzielanie skrzynek;
- obsługa wiadomości przychodzących;
- kontrola dostępu do skrzynki;
- możliwość zachowania danych zgodnie z wybraną polityką.

Wybór rozwiązania będzie zależeć od dostępnych API, kosztów, bezpieczeństwa i warunków dostawcy.

### 3.5. Moduł numerów telefonicznych

Docelowo platforma będzie korzystać z własnej puli numerów telefonicznych obsługujących wiadomości SMS.

Rozważane funkcje:
- ewidencja dostępnych numerów;
- sprawdzanie ich dostępności;
- czasowe przypisywanie numerów do sesji;
- możliwość wykupienia numeru na wyłączność;
- odbieranie i wyświetlanie wiadomości;
- rozliczanie wykorzystania numerów;
- zwalnianie numeru po zakończeniu przypisania.

Numery współdzielone i numery na wyłączność będą wymagały osobnych zasad zarządzania. Dostępność SMS-ów zależy od operatora i może różnić się w zależności od serwisu docelowego.

### 3.6. Moduł kont i ustawień

Model kont pozostaje otwarty na etapie projektowania.

Rozważane warianty:
- anonimowe korzystanie bez rejestracji;
- opcjonalne konto do zachowywania ustawień;
- konto z możliwością zarządzania skrzynkami i przypisanymi numerami.

Wybór powinien uwzględniać kompromis pomiędzy wygodą, odzyskiwaniem dostępu, prywatnością i kosztami utrzymania.

### 3.7. Moduł weryfikacji

Platforma powinna umożliwiać użytkownikowi samodzielne przejście wymaganych etapów weryfikacji.

Założenia:
- obsługa CAPTCHA w zgodnym z usługą przepływie;
- ręczne rozwiązywanie CAPTCHA przez użytkownika;
- obsługa kodów otrzymanych e-mailem lub SMS-em;
- brak automatycznego omijania zabezpieczeń antybotowych.

## 4. Prywatność i model zagrożeń

Prywatność należy traktować jako właściwość całego systemu, a nie tylko obecność serwera proxy.

Przed wdrożeniem należy określić:
- jakie dane są widoczne dla platformy;
- jakie dane otrzymuje strona docelowa;
- czy i w jakim zakresie przechowywane są adresy IP i logi;
- jak izolowane są sesje użytkowników;
- jak długo przechowywane są wiadomości e-mail i SMS;
- jak chronione są dane dostępowe;
- jakie dane mogą być udostępniane dostawcom zewnętrznym.

Należy również pamiętać, że proxy samo w sobie nie eliminuje identyfikacji przez pliki cookies, fingerprinting przeglądarki, logowanie na osobiste konto ani inne mechanizmy śledzenia.

## 5. Wstępna architektura

Docelowy podział logiczny może wyglądać następująco:

- **Frontend** — interfejs użytkownika.
- **Backend API** — obsługa sesji, ustawień i logiki aplikacji.
- **Proxy Gateway** — kontrolowane przekazywanie ruchu.
- **Email Service** — integracja z dostawcą poczty.
- **SMS Service** — zarządzanie pulą numerów i odbieraniem wiadomości.
- **Data Store** — przechowywanie minimalnego zakresu niezbędnych danych.
- **Security Layer** — uwierzytelnianie, autoryzacja, limity i kontrola dostępu.

Są to moduły logiczne, a nie ostateczna decyzja o liczbie serwisów lub użytych technologiach. Konkretna implementacja zostanie ustalona po wyborze stosu technologicznego.

## 6. Plan realizacji MVP

### Etap 1 — projekt i wymagania
- określenie modelu działania proxy;
- wybór obsługiwanych protokołów;
- zdefiniowanie modelu sesji;
- określenie zasad prywatności;
- wybór technologii.

### Etap 2 — podstawowa aplikacja
- przygotowanie interfejsu;
- implementacja backendu;
- obsługa wprowadzania i walidacji adresów URL;
- podstawowe zarządzanie sesjami.

### Etap 3 — prototyp proxy
- przekazywanie ruchu dla wybranego zakresu stron;
- kontrola dostępu do adresów docelowych;
- izolacja sesji;
- testy poprawności i bezpieczeństwa.

### Etap 4 — generowanie danych
- generator identyfikatorów;
- generator haseł;
- tymczasowe przechowywanie danych;
- bezpieczne usuwanie danych po zakończeniu sesji.

### Etap 5 — poczta elektroniczna
- wybór dostawcy;
- integracja z API;
- odbieranie i prezentowanie wiadomości;
- testy bezpieczeństwa i retencji.

### Etap 6 — testy i dokumentacja
- testy funkcjonalne;
- testy izolacji sesji;
- testy ochrony przed nieautoryzowanym dostępem;
- dokumentacja ograniczeń;
- przygotowanie pierwszej wersji demonstracyjnej.

### Po MVP
- obsługa numerów telefonicznych;
- możliwość wykupienia numeru na wyłączność;
- integracja płatności;
- rozwój modelu kont;
- dodatkowe mechanizmy prywatności i zarządzania sesjami.

## 7. Najważniejsze ryzyka techniczne

- Nie wszystkie strony będą poprawnie działać przez wybrany typ proxy.
- Dostawcy poczty mogą ograniczać automatyczne tworzenie skrzynek.
- Numery telefoniczne mogą nie odbierać wiadomości ze wszystkich serwisów.
- Obsługa wielu sesji zwiększa wymagania dotyczące izolacji i bezpieczeństwa.
- Niewłaściwie skonfigurowane proxy może zostać wykorzystane do nieautoryzowanego dostępu do zasobów sieciowych.
- Utrzymywanie poczty, numerów i ruchu sieciowego może generować koszty operacyjne.
- Pełnej anonimowości nie można zagwarantować samym pośredniczeniem w ruchu.

## 8. Kryteria ukończenia MVP

MVP można uznać za gotowe do demonstracji, gdy:
- użytkownik może otworzyć interfejs i rozpocząć obsługiwaną sesję;
- proxy działa dla określonego, przetestowanego zakresu stron;
- sesje są od siebie odpowiednio odizolowane;
- użytkownik może wygenerować identyfikator i silne hasło;
- działa co najmniej jedna integracja pocztowa, o ile została objęta zakresem pierwszego wydania;
- dane tymczasowe są usuwane zgodnie z ustalonymi zasadami;
- podstawowe testy bezpieczeństwa kończą się pomyślnie;
- dokumentacja jasno opisuje ograniczenia rozwiązania.

## 9. Aktualny status

**Status:** koncepcja i planowanie MVP.

**Do ustalenia:**
- stos technologiczny;
- dokładny typ proxy;
- zakres obsługiwanych stron;
- model kont i przechowywania danych;
- dostawca poczty;
- sposób finansowania i późniejsza obsługa numerów telefonicznych.

Projekt będzie realiz
