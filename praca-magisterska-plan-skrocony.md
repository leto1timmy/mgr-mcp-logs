# Skrócony plan pracy magisterskiej

## Temat

**Projekt, implementacja i ewaluacja serwera Model Context Protocol do wspomaganej przez LLM analizy logów w systemach mikroserwisowych**

## Uzasadnienie wyboru tematu

Systemy mikroserwisowe składają się z wielu niezależnie rozwijanych i wdrażanych usług. Taka architektura ułatwia skalowanie i rozwój systemu, ale utrudnia analizę awarii, ponieważ pojedynczy incydent może obejmować wiele usług, wiele wpisów logów i różne źródła danych telemetrycznych. Klasyczne narzędzia observability, takie jak Elasticsearch, Grafana Loki czy OpenTelemetry, dostarczają mechanizmów wyszukiwania i korelacji danych, lecz wymagają znajomości struktury systemu oraz składni zapytań.

Model Context Protocol (MCP) umożliwia udostępnianie narzędzi i danych aplikacjom wykorzystującym duże modele językowe. W pracy zostanie zbadane, czy serwer MCP może stanowić użyteczną warstwę dostępu do logów mikroserwisów i wspierać użytkownika w analizie incydentów za pomocą języka naturalnego.

## Cel pracy

Celem pracy jest zaprojektowanie, zaimplementowanie i ocena prototypowego serwera MCP wspierającego analizę logów w środowisku mikroserwisowym.

Cele szczegółowe:

- analiza architektury MCP i jej zastosowania w obszarze observability;
- opracowanie modelu danych logów z obsługą `trace_id` i `span_id`;
- zaprojektowanie zestawu narzędzi MCP do wyszukiwania, agregacji i interpretacji logów;
- implementacja prototypu serwera MCP w TypeScript;
- przygotowanie testowego środowiska mikroserwisowego;
- ocena skuteczności, wydajności i ograniczeń rozwiązania;
- analiza ryzyk bezpieczeństwa związanych z udostępnianiem logów klientom LLM.

## Problem badawczy

Praca odpowiada na pytanie, czy serwer MCP może pełnić rolę bezpiecznej i użytecznej warstwy pośredniej między systemem logowania a klientem LLM oraz w jakich scenariuszach takie podejście jest korzystniejsze od tradycyjnej, ręcznej analizy logów.

## Pytania badawcze

1. Czy serwer MCP może uprościć analizę incydentów w systemie mikroserwisowym?
2. Jakie typy zadań diagnostycznych najlepiej nadają się do obsługi przez narzędzia MCP?
3. Jak strukturalne logi oraz identyfikatory `trace_id` i `span_id` wpływają na jakość analizy?
4. Jakie ograniczenia ma LLM jako interfejs do danych operacyjnych?
5. Jakie mechanizmy bezpieczeństwa są potrzebne przy udostępnianiu logów przez MCP?
6. Jak MCP wypada na tle REST API, OpenAPI/function calling, RAG i klasycznych dashboardów observability?

## Zakres pracy

Praca obejmuje projekt i implementację prototypowego serwera MCP, narzędzia do odczytu i analizy logów, środowisko testowe z kilkoma mikroserwisami oraz ewaluację rozwiązania na przygotowanych scenariuszach diagnostycznych. Praca nie obejmuje trenowania własnego modelu językowego, budowy pełnej platformy observability ani automatycznego wykonywania działań naprawczych w systemie produkcyjnym.

## Planowana struktura pracy

### 1. Wstęp

Rozdział przedstawi motywację, cel pracy, problem badawczy, pytania badawcze, zakres pracy oraz strukturę dokumentu.

### 2. Podstawy teoretyczne i analiza literatury

Rozdział omówi architekturę mikroserwisową, observability, logi strukturalne, korelację logów z trace'ami, rolę LLM jako interfejsu analitycznego, architekturę MCP oraz ryzyka bezpieczeństwa związane z aplikacjami agentowymi.

### 3. Analiza problemu i wymagań

Rozdział opisze scenariusze diagnostyczne, wymagania funkcjonalne i niefunkcjonalne, przypadki użycia oraz ryzyka związane z udostępnianiem danych z logów klientom AI.

### 4. Projekt systemu

Rozdział przedstawi architekturę rozwiązania, model danych logów, projekt narzędzi MCP, zasoby MCP, szablony promptów, mechanizmy bezpieczeństwa oraz uzasadnienie wyboru technologii.

### 5. Implementacja

Rozdział opisze strukturę projektu, konfigurację serwera MCP, implementację narzędzi, walidację wejścia, schematy wyjścia, obsługę błędów, maskowanie danych oraz testy.

### 6. Ewaluacja i analiza wyników

Rozdział przedstawi środowisko testowe, metodykę ewaluacji, metryki ilościowe i jakościowe, analizę bezpieczeństwa, analizę wydajności oraz dyskusję wyników w odniesieniu do pytań badawczych.

### 7. Podsumowanie

Rozdział podsumuje osiągnięte cele, odpowie na pytania badawcze, wskaże ograniczenia pracy oraz możliwe kierunki dalszego rozwoju.

## Planowany stos technologiczny

| Obszar | Technologia |
| ------ | ----------- |
| Serwer MCP | TypeScript, `@modelcontextprotocol/sdk` |
| Transport MCP | `stdio`; Streamable HTTP jako wariant analityczny lub rozszerzenie |
| Backend logów | PostgreSQL `jsonb` w prototypie; porównanie z Elasticsearch i Loki |
| Dane telemetryczne | logi strukturalne z `trace_id` i `span_id` |
| Środowisko testowe | Docker Compose, kilka przykładowych mikroserwisów |
| Klient AI | klient obsługujący MCP, np. Claude Desktop lub Cursor |

## Planowane narzędzia MCP

| Narzędzie | Cel |
| --------- | --- |
| `search_logs` | wyszukiwanie logów po czasie, usłudze, poziomie i treści |
| `get_trace` | pobranie logów powiązanych z jednym `trace_id` |
| `error_summary` | agregacja błędów w wybranym przedziale czasu |
| `service_health` | podsumowanie stanu wybranej usługi |
| `find_anomalies` | wykrywanie nietypowych wzorców w logach |
| `top_error_patterns` | grupowanie najczęstszych wzorców błędów |
| `compare_time_windows` | porównanie dwóch przedziałów czasowych |
| `explain_error_context` | zebranie kontekstu błędu dla użytkownika |
| `audit_tool_usage` | audyt wywołań narzędzi MCP |

## Oczekiwany rezultat

Rezultatem pracy będzie prototyp serwera MCP, opis jego architektury i implementacji, zestaw scenariuszy testowych oraz ocena przydatności MCP jako interfejsu do analizy logów w środowisku mikroserwisowym.
