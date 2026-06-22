# Plan pracy magisterskiej

## Temat

**"Projekt, implementacja i ewaluacja serwera Model Context Protocol do wspomaganej przez LLM analizy logów w systemach mikroserwisowych"**

Temat został doprecyzowany względem pierwotnej wersji, aby poza implementacją wyraźnie obejmował część analityczną i ewaluacyjną. Dzięki temu praca nie sprowadza się tylko do opisu stworzonego narzędzia, lecz bada przydatność MCP jako warstwy integracyjnej między systemem observability a asystentem AI.

---

## Problem badawczy

W systemach mikroserwisowych analiza incydentów wymaga korelowania danych pochodzących z wielu usług, przedziałów czasowych i źródeł telemetrycznych. Klasyczne narzędzia observability udostępniają zaawansowane mechanizmy wyszukiwania i wizualizacji, ale wymagają znajomości składni zapytań, struktury danych i architektury systemu. Model Context Protocol umożliwia wystawienie danych i operacji analitycznych w ustandaryzowany sposób dla klientów LLM, jednak wprowadza też nowe ryzyka: błędną interpretację wyników, nadmierną autonomię narzędzi, wyciek danych oraz podatność na prompt injection i tool poisoning.

Praca analizuje, czy serwer MCP może pełnić rolę bezpiecznej i użytecznej warstwy dostępu do logów mikroserwisów oraz jakie ograniczenia ma takie podejście w porównaniu z tradycyjnymi narzędziami do analizy logów.

---

## Cel pracy

Celem głównym pracy jest zaprojektowanie, zaimplementowanie i ocena prototypu serwera MCP wspierającego analizę logów w środowisku mikroserwisowym.

Cele szczegółowe:

- analiza architektury MCP i jej dopasowania do problemu observability;
- opracowanie modelu danych logów umożliwiającego korelację po czasie, usłudze, poziomie błędu i identyfikatorze śladu;
- zaprojektowanie zestawu narzędzi MCP dla typowych zadań diagnostycznych;
- implementacja prototypu serwera MCP w TypeScript;
- przygotowanie testowego środowiska mikroserwisowego generującego logi i scenariusze awarii;
- ewaluacja skuteczności, wydajności i ograniczeń rozwiązania;
- analiza ryzyk bezpieczeństwa wynikających z udostępniania logów klientom LLM.

---

## Pytania badawcze

1. Czy serwer MCP może skrócić i uprościć analizę incydentów w systemie mikroserwisowym w porównaniu z ręcznym wyszukiwaniem logów?
2. Jakie typy zapytań diagnostycznych najlepiej nadają się do obsługi przez narzędzia MCP?
3. Jak strukturalne logi, `trace_id` i `span_id` wpływają na jakość odpowiedzi generowanych przez klienta LLM?
4. Jakie ograniczenia ma LLM jako interfejs do danych operacyjnych?
5. Jakie mechanizmy bezpieczeństwa powinien mieć serwer MCP udostępniający logi aplikacyjne?
6. Jak MCP wypada na tle alternatywnych podejść: REST API, OpenAPI/function calling, RAG oraz klasycznych dashboardów observability?

---

## Zakres pracy

Praca obejmuje:

- projekt i implementację prototypowego serwera MCP;
- analizę logów z przykładowego środowiska mikroserwisowego;
- narzędzia MCP wyłącznie do odczytu i analizy danych;
- podstawowy model bezpieczeństwa: walidacja wejścia, ograniczenie zakresu zapytań, maskowanie danych wrażliwych, audyt wywołań;
- ewaluację na przygotowanych scenariuszach diagnostycznych.

Praca nie obejmuje:

- budowy kompletnej platformy observability klasy produkcyjnej;
- trenowania własnego modelu językowego;
- pełnej integracji z wieloma dostawcami LLM;
- automatycznego wykonywania działań naprawczych w systemie mikroserwisowym.

---

## Wymogi formalne PJATK

| Element      | Wymaganie                                      |
| ------------ | ---------------------------------------------- |
| Objętość     | min. **72 000 znaków** ze spacjami (~40 stron) |
| Czcionka     | Times New Roman 12 pt                          |
| Interlinia   | 1,5                                            |
| Marginesy    | 2,5 cm                                         |
| Streszczenie | max. 1000 znaków + 3-5 słów kluczowych         |
| Przypisy     | dolne, format wg wzoru PJATK                   |
| Oprawa       | twarda (granat/czerń/bordo/zieleń)             |

---

## Struktura pracy

### 1. Wstęp (~3 strony)

- Uzasadnienie wyboru tematu
- Problem badawczy
- Cel główny i cele szczegółowe
- Pytania badawcze
- Zakres pracy
- Struktura dokumentu

### 2. Podstawy teoretyczne i analiza literatury (~16-18 stron)

Ten rozdział powinien bezpośrednio odpowiadać na uwagę promotora dotyczącą zwiększenia treści analitycznych i teoretycznych.

- 2.1. Architektura mikroserwisowa
  - dekompozycja systemu na usługi;
  - komunikacja synchroniczna i asynchroniczna;
  - typowe źródła awarii w systemach rozproszonych;
  - wpływ mikroserwisów na trudność diagnostyki.
- 2.2. Observability w systemach rozproszonych
  - różnica między monitoringiem a observability;
  - logi, metryki i trace'y jako sygnały telemetryczne;
  - korelacja danych telemetrycznych;
  - pojęcia SLI, SLO i analiza incydentów.
- 2.3. Logi aplikacyjne jako źródło danych diagnostycznych
  - logi tekstowe a logi strukturalne;
  - pola logu: czas, poziom, usługa, komunikat, `trace_id`, `span_id`, metadane środowiska;
  - problemy jakości logów: brak kontekstu, nadmiar danych, niespójne formaty, dane wrażliwe;
  - kardynalność etykiet i konsekwencje dla wydajności systemów logowania.
- 2.4. Narzędzia i standardy observability
  - Elasticsearch/ELK;
  - Grafana Loki;
  - OpenTelemetry i model korelacji logów z trace'ami;
  - porównanie podejść do indeksowania i wyszukiwania logów.
- 2.5. Duże modele językowe jako interfejs analityczny
  - LLM jako warstwa interpretacji danych technicznych;
  - function calling i tool use;
  - ograniczenia: halucynacje, zależność od jakości kontekstu, nadmierne zaufanie użytkownika;
  - rola człowieka w pętli decyzyjnej.
- 2.6. Model Context Protocol
  - geneza MCP i problem fragmentacji integracji;
  - role: host, client, server;
  - format komunikacji JSON-RPC;
  - narzędzia (`tools`), zasoby (`resources`) i szablony promptów (`prompts`);
  - transport `stdio` oraz Streamable HTTP;
  - negocjacja możliwości i wersjonowanie protokołu;
  - schematy wejścia i wyjścia narzędzi;
  - znaczenie rejestrów MCP i ekosystemu serwerów.
- 2.7. Bezpieczeństwo MCP i aplikacji agentowych
  - zgoda użytkownika i kontrola nad wywołaniami narzędzi;
  - prompt injection i indirect prompt injection;
  - tool poisoning przez opisy narzędzi i metadane;
  - excessive agency;
  - wyciek danych z logów;
  - modelowanie zagrożeń z użyciem STRIDE;
  - znaczenie walidacji wejścia, kontroli dostępu, limitów i audytu.
- 2.8. MCP na tle alternatywnych podejść
  - MCP vs REST API;
  - MCP vs OpenAPI/function calling;
  - MCP vs RAG;
  - MCP vs klasyczne dashboardy Kibana/Grafana;
  - lokalny serwer MCP `stdio` vs zdalny serwer MCP przez HTTP.

Źródła robocze dla rozdziału 2 są śledzone w pliku [praca-magisterska-log-zmian-i-zrodla.md](/Users/leto/Dev/PJATK/MGR/praca-magisterska-log-zmian-i-zrodla.md).

### 3. Analiza problemu i wymagań (~7 stron)

- 3.1. Charakterystyka analizowanego problemu
  - typowy przebieg incydentu w środowisku mikroserwisowym;
  - potrzeby użytkownika: programista, DevOps, SRE;
  - ograniczenia ręcznej analizy logów.
- 3.2. Scenariusze diagnostyczne
  - wzrost liczby błędów HTTP 5xx;
  - błąd w jednym serwisie propagujący się do kolejnych;
  - wolne odpowiedzi i timeouty;
  - analiza konkretnego `trace_id`;
  - porównanie zachowania systemu przed i po wdrożeniu.
- 3.3. Wymagania funkcjonalne
  - wyszukiwanie logów po czasie, usłudze, poziomie i treści;
  - pobieranie logów powiązanych z trace'em;
  - agregacja błędów;
  - analiza najczęstszych wzorców błędów;
  - porównywanie okien czasowych;
  - raportowanie stanu usług;
  - audyt wywołań narzędzi MCP.
- 3.4. Wymagania niefunkcjonalne
  - ograniczony czas odpowiedzi narzędzi;
  - walidacja parametrów wejściowych;
  - limity zakresu czasowego i liczby wyników;
  - maskowanie danych wrażliwych;
  - czytelność odpowiedzi dla LLM i użytkownika;
  - powtarzalność testów.
- 3.5. Analiza ryzyka
  - ryzyko ujawnienia danych wrażliwych z logów;
  - ryzyko błędnej interpretacji przez LLM;
  - ryzyko zbyt szerokich uprawnień narzędzi;
  - ryzyko prompt injection w treści logów;
  - ryzyko tool poisoning po stronie serwera lub klienta.
- 3.6. Przypadki użycia
  - diagram przypadków użycia UML;
  - opis aktorów;
  - warunki początkowe, scenariusz główny i scenariusze alternatywne.

### 4. Projekt systemu (~9 stron)

- 4.1. Architektura rozwiązania
  - diagram komponentów;
  - relacje między klientem AI, serwerem MCP, bazą logów i środowiskiem testowym;
  - wariant lokalny `stdio` i wariant zdalny Streamable HTTP jako analiza projektowa.
- 4.2. Model danych logów
  - struktura rekordu logu;
  - indeksowanie i pola wyszukiwania;
  - obsługa `trace_id` i `span_id`;
  - pola przeznaczone do maskowania.
- 4.3. Definicja narzędzi MCP
  - nazwy narzędzi i ich opisy;
  - schematy wejściowe;
  - schematy wyjściowe;
  - klasyfikacja narzędzi według ryzyka;
  - zasady obsługi błędów wykonania narzędzia.
- 4.4. Definicja zasobów MCP
  - dokumentacja modelu danych jako resource;
  - przykładowe zapytania diagnostyczne;
  - opis środowiska testowego;
  - lista dostępnych usług.
- 4.5. Szablony promptów MCP
  - analiza incydentu;
  - wyjaśnienie błędu;
  - podsumowanie stanu usługi;
  - porównanie dwóch przedziałów czasowych.
- 4.6. Bezpieczeństwo i autoryzacja
  - model narzędzi tylko do odczytu;
  - limity zapytań;
  - maskowanie danych;
  - audyt wywołań;
  - polityka zatwierdzania narzędzi w kliencie AI.
- 4.7. Wybór technologii
  - uzasadnienie wyboru TypeScript i SDK MCP;
  - wybór backendu logów;
  - uzasadnienie środowiska Docker Compose;
  - kryteria porównawcze wobec Elasticsearch i Loki.

### 5. Implementacja (~10 stron)

- 5.1. Struktura projektu
- 5.2. Konfiguracja serwera MCP
- 5.3. Implementacja narzędzi MCP
  - `search_logs`;
  - `get_trace`;
  - `error_summary`;
  - `service_health`;
  - `find_anomalies`;
  - `top_error_patterns`;
  - `compare_time_windows`;
  - `explain_error_context`;
  - `audit_tool_usage`.
- 5.4. Implementacja zasobów i promptów MCP
- 5.5. Walidacja wejścia i schematy wyjścia
- 5.6. Obsługa błędów
  - błędy protokołu;
  - błędy wykonania narzędzia;
  - komunikaty umożliwiające samokorektę przez LLM.
- 5.7. Bezpieczeństwo implementacji
  - sanityzacja wyników;
  - maskowanie danych;
  - limity zakresów czasowych;
  - rejestrowanie wywołań.
- 5.8. Testy jednostkowe i integracyjne
- 5.9. Napotkane problemy i ich rozwiązania

### 6. Ewaluacja i analiza wyników (~10 stron)

Ten rozdział zastępuje wcześniejszą "Prezentację rozwiązania". Demonstracja nadal może wystąpić, ale jako materiał pomocniczy do oceny, nie jako główna treść rozdziału.

- 6.1. Środowisko testowe
  - Docker Compose z 3-4 mikroserwisami;
  - generator ruchu;
  - scenariusze awarii;
  - zbiór testowych logów.
- 6.2. Metodyka ewaluacji
  - opis zadań diagnostycznych;
  - porównanie pracy z MCP i bez MCP;
  - kryteria poprawności odpowiedzi;
  - powtarzalność eksperymentu.
- 6.3. Metryki ilościowe
  - czas wykonania zapytania;
  - liczba wywołań narzędzi;
  - liczba zwróconych logów;
  - skuteczność odnalezienia właściwej przyczyny;
  - liczba błędnych lub niepełnych odpowiedzi;
  - koszt kontekstowy: rozmiar danych przekazywanych do LLM.
- 6.4. Ocena jakościowa
  - użyteczność odpowiedzi;
  - czytelność podsumowań;
  - zdolność do wskazania dalszych kroków diagnostycznych;
  - przypadki, w których LLM wymaga korekty użytkownika.
- 6.5. Analiza bezpieczeństwa
  - testy prompt injection w treści logów;
  - próby przekroczenia dozwolonego zakresu czasu;
  - próby wyszukania danych wrażliwych;
  - ocena skuteczności maskowania i audytu.
- 6.6. Analiza wydajności
  - wpływ liczby logów na czas odpowiedzi;
  - wpływ agregacji na obciążenie backendu;
  - ograniczenia prototypu.
- 6.7. Dyskusja wyników
  - odpowiedzi na pytania badawcze;
  - interpretacja wyników;
  - porównanie z alternatywnymi podejściami;
  - ograniczenia ważności wniosków.

### 7. Podsumowanie (~3 strony)

- Osiągnięte cele
- Odpowiedzi na pytania badawcze
- Wnioski projektowe i badawcze
- Ograniczenia pracy
- Możliwości dalszego rozwoju:
  - pełna obsługa zdalnego transportu HTTP;
  - integracja z Elasticsearch lub Loki;
  - rozbudowany model uprawnień;
  - obsługa metryk i trace'ów, nie tylko logów;
  - benchmark na większym zbiorze danych.

### Bibliografia

Bibliografia będzie rozwijana na podstawie rejestru źródeł prowadzonego w pliku [praca-magisterska-log-zmian-i-zrodla.md](/Users/leto/Dev/PJATK/MGR/praca-magisterska-log-zmian-i-zrodla.md).

### Wykaz rysunków

Planowane rysunki:

- architektura mikroserwisowa środowiska testowego;
- przepływ zapytania przez klienta AI, MCP i backend logów;
- model danych logów;
- diagram przypadków użycia;
- diagram komponentów;
- diagram sekwencji dla wywołania narzędzia MCP;
- diagram modelu zagrożeń.

### Wykaz tabel

Planowane tabele:

- porównanie MCP, REST API, function calling, RAG i dashboardów observability;
- wymagania funkcjonalne;
- wymagania niefunkcjonalne;
- narzędzia MCP i ich schematy;
- klasyfikacja ryzyka narzędzi;
- metryki ewaluacyjne;
- wyniki eksperymentów.

### Załączniki (opcjonalnie)

- fragmenty kodu;
- pełne schematy JSON narzędzi;
- przykładowe logi;
- przykładowe rozmowy z klientem AI;
- dodatkowe wyniki testów.

---

## Planowany stos technologiczny

| Warstwa            | Technologia / założenie                                                |
| ------------------ | ---------------------------------------------------------------------- |
| Serwer MCP         | TypeScript + `@modelcontextprotocol/sdk`                               |
| Transport MCP      | `stdio` jako wariant implementowany, Streamable HTTP jako wariant analityczny lub rozszerzenie |
| Backend logów      | PostgreSQL `jsonb` dla prototypu; porównanie założeń z Elasticsearch i Loki |
| Dane telemetryczne | Logi strukturalne z polami `trace_id` i `span_id`                      |
| Testowe środowisko | Docker Compose z 3-4 mikroserwisami                                    |
| Klient AI          | Claude Desktop, Cursor lub inny klient obsługujący MCP                 |
| Testy              | Testy jednostkowe i integracyjne narzędzi MCP                          |

---

## Narzędzia MCP do zaimplementowania

| Tool                   | Opis                                                                 | Typ wyniku |
| ---------------------- | -------------------------------------------------------------------- | ---------- |
| `search_logs`          | Wyszukiwanie logów po czasie, poziomie, serwisie i treści            | lista logów + metadane zapytania |
| `get_trace`            | Pobranie logów dla danego `trace_id`                                  | uporządkowana oś zdarzeń |
| `error_summary`        | Agregacja błędów z ostatnich N minut/godzin                           | statystyki i najczęstsze błędy |
| `service_health`       | Status i statystyki dla konkretnego mikroserwisu                      | podsumowanie stanu usługi |
| `find_anomalies`       | Wykrycie nietypowych wzorców, np. skoku błędów                        | lista anomalii z uzasadnieniem |
| `top_error_patterns`   | Grupowanie błędów po wzorcu komunikatu, statusie i usłudze            | ranking wzorców błędów |
| `compare_time_windows` | Porównanie dwóch przedziałów czasowych, np. przed i po wdrożeniu      | różnice w liczbie błędów i wzorcach |
| `explain_error_context` | Zebranie kontekstu błędu: poprzednie logi, usługa, trace, możliwa przyczyna | narracyjne podsumowanie + dane źródłowe |
| `audit_tool_usage`     | Historia wywołań narzędzi MCP, argumenty i zakres udostępnionych danych | audyt bezpieczeństwa |

Każde narzędzie powinno mieć jawny schemat wejściowy, ograniczenia zakresu zapytania, schemat wyjściowy oraz zdefiniowany sposób zgłaszania błędów wykonania.

---

## Zasady pracy nad źródłami

- Każde istotne źródło otrzymuje identyfikator w rejestrze źródeł.
- Każda większa zmiana planu powinna być wpisana do logu zmian.
- Przy każdym źródle należy zapisać: link, typ źródła, datę dostępu, powiązane rozdziały i krótką notatkę, do czego źródło może zostać użyte.
- Źródła techniczne i dokumentacyjne powinny być uzupełnione literaturą naukową, szczególnie w rozdziałach dotyczących bezpieczeństwa, observability i systemów agentowych.
