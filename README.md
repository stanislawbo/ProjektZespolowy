# Privacy Gateway

**Platforma do prywatnego przeglądania internetu z wykorzystaniem serwera pośredniczącego (proxy).**

## Opis projektu

Privacy Gateway to projekt platformy internetowej, której celem jest zwiększenie prywatności użytkowników podczas korzystania z internetu. System ma działać jako pośrednik pomiędzy użytkownikiem a odwiedzanymi stronami internetowymi, oferując dodatkowo narzędzia do zarządzania danymi kont, obsługi poczty elektronicznej oraz w przyszłości korzystania z numerów telefonicznych do weryfikacji.

Projekt będzie rozwijany etapami, zaczynając od wersji MVP (Minimum Viable Product), która pozwoli przetestować podstawowe założenia techniczne i funkcjonalne.

## Główne cele

- Zapewnienie prywatniejszego dostępu do stron internetowych przez serwer pośredniczący.
- Ograniczenie gromadzenia danych użytkowników do niezbędnego minimum.
- Umożliwienie generowania losowych identyfikatorów, nazw użytkownika i silnych haseł.
- Integracja z usługami poczty elektronicznej.
- Opracowanie systemu obsługi numerów telefonicznych do weryfikacji, z możliwością przypisania numeru na wyłączność.
- Zapewnienie przejrzystego i prostego interfejsu użytkownika.

## Planowane funkcjonalności

### 1. Przeglądanie przez proxy
- Przekazywanie obsługiwanych żądań internetowych przez serwer pośredniczący.
- Interfejs umożliwiający podanie adresu strony internetowej.
- Zarządzanie sesją przeglądania.
- Rozważenie izolacji sesji, plików cookies i danych poszczególnych użytkowników.
- Opracowanie polityki przechowywania i usuwania logów.

### 2. Generowanie danych użytkownika
- Automatyczne generowanie losowego identyfikatora.
- Generowanie silnych, losowych haseł.
- Opcjonalne użycie danych podanych przez użytkownika.
- Bezpieczne przechowywanie danych, jeśli użytkownik zdecyduje się je zapisać.

### 3. Obsługa poczty elektronicznej
- Integracja z tymczasowymi skrzynkami e-mail.
- Możliwość integracji z usługą tworzącą oddzielne skrzynki dla użytkowników.
- Odbieranie wiadomości i kodów weryfikacyjnych.
- Osobny widok skrzynki odbiorczej.
- Bezpieczne zarządzanie danymi dostępowymi.

Dostępność automatycznego tworzenia skrzynek będzie zależeć od możliwości i warunków wybranego dostawcy.

### 4. Obsługa numerów telefonicznych
- Własna pula numerów telefonicznych.
- Możliwość czasowego przypisania numeru lub wynajęcia go na wyłączność.
- Oddzielna zakładka do wyświetlania wiadomości SMS związanych z obsługiwanymi usługami.
- Możliwość rozliczania płatnego dostępu do numerów w kolejnych etapach rozwoju.

Funkcja będzie zależeć od dostępności numerów, obsługi SMS oraz wymagań prawnych i operatorów.

### 5. Weryfikacja użytkownika
- Obsługa CAPTCHA, np. reCAPTCHA, w miejscach, w których jest wymagana.
- Rozwiązywanie CAPTCHA przez użytkownika.
- Brak automatycznego omijania mechanizmów antybotowych.

### 6. Konto na platformie
Model rejestracji pozostaje do ustalenia. Rozważane są dwa warianty:
- korzystanie z podstawowych funkcji bez rejestracji;
- utworzenie konta pozwalającego zapisywać ustawienia i zarządzać własnymi danymi.

