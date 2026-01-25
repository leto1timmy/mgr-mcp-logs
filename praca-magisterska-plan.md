# Plan pracy magisterskiej

## Temat

**"Projekt i implementacja serwera MCP do analizy logów w systemach mikroserwisowych"**

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
- Cel pracy: zaprojektowanie i zaimplementowanie serwera MCP
- Zakres pracy
- Struktura dokumentu

### 2. Podstawy teoretyczne (~12 stron)

- 2.1. Architektura mikroserwisowa i wyzwania observability
- 2.2. Systemy logowania w środowiskach rozproszonych (ELK, Loki, OpenTelemetry)
- 2.3. Model Context Protocol (MCP) – specyfikacja i architektura
- 2.4. Duże modele językowe jako narzędzia wspierające pracę programisty

### 3. Analiza wymagań (~5 stron)

- 3.1. Wymagania funkcjonalne
- 3.2. Wymagania niefunkcjonalne
- 3.3. Przypadki użycia (diagramy UML)

### 4. Projekt systemu (~8 stron)

- 4.1. Architektura rozwiązania (diagram komponentów)
- 4.2. Model danych logów
- 4.3. Definicja narzędzi MCP (tools) i zasobów (resources)
- 4.4. Integracja z klientami AI (Claude Desktop, Cursor)
- 4.5. Wybór technologii

### 5. Implementacja (~10 stron)

- 5.1. Struktura projektu
- 5.2. Konfiguracja serwera MCP
- 5.3. Implementacja narzędzi:
  - Wyszukiwanie logów
  - Filtrowanie i agregacja
  - Korelacja trace ID
  - Analiza błędów
- 5.4. Obsługa bezpieczeństwa i autoryzacji
- 5.5. Napotkane problemy i ich rozwiązania

### 6. Prezentacja rozwiązania (~8 stron)

- 6.1. Środowisko testowe (Docker Compose z przykładowymi mikroserwisami)
- 6.2. Scenariusze użycia z przykładami konwersacji
- 6.3. Demonstracja kluczowych funkcjonalności (zrzuty ekranu)
- 6.4. Analiza wydajności i ograniczenia

### 7. Podsumowanie (~2 strony)

- Osiągnięte cele
- Wnioski
- Możliwości dalszego rozwoju

### Bibliografia

### Wykaz rysunków

### Wykaz tabel

### Załączniki (opcjonalnie)

- Fragmenty kodu
- Diagramy

---

## Planowany stos technologiczny

| Warstwa            | Technologia                            |
| ------------------ | -------------------------------------- |
| Serwer MCP         | TypeScript + @modelcontextprotocol/sdk |
| Backend logów      | Elasticsearch lub PostgreSQL (jsonb)   |
| Testowe środowisko | Docker Compose z 3-4 mikroserwisami    |
| Klient AI          | Claude Desktop lub Cursor IDE          |

---

## Narzędzia MCP do zaimplementowania

| Tool             | Opis                                                     |
| ---------------- | -------------------------------------------------------- |
| `search_logs`    | Wyszukiwanie logów po czasie, poziomie, serwisie, treści |
| `get_trace`      | Pobranie wszystkich logów dla danego trace ID            |
| `error_summary`  | Agregacja błędów z ostatnich N minut/godzin              |
| `service_health` | Status i statystyki dla konkretnego mikroserwisu         |
| `find_anomalies` | Wykrycie nietypowych wzorców (np. spike błędów)          |
