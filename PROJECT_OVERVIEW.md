# Privacy Gateway — ogólny zarys projektu

## 1. Wizja projektu

Celem projektu Privacy Gateway - PriG jest stworzenie platformy internetowej, która pozwoli użytkownikom korzystać z wybranych zasobów internetu za pośrednictwem serwera pośredniczącego, przy jednoczesnym ograniczeniu ilości udostępnianych danych osobowych.

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

