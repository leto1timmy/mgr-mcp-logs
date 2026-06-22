# Skrócony plan pracy magisterskiej

## Temat

**Projekt i implementacja serwera MCP do analizy logów w systemach mikroserwisowych**

## Cel pracy

Celem pracy jest zaprojektowanie i zaimplementowanie prototypowego serwera zgodnego z protokołem MCP, który będzie wspierał analizę logów w środowisku mikroserwisowym. Praca obejmuje również analizę teoretyczną problemu, projekt rozwiązania, implementację narzędzi MCP oraz ocenę działania prototypu na przygotowanych scenariuszach diagnostycznych.

## Proponowany spis treści

### 1. Wstęp

1.1. Uzasadnienie wyboru tematu  
1.2. Cel i zakres pracy  
1.3. Problem badawczy  
1.4. Pytania badawcze  
1.5. Metodyka realizacji pracy  
1.6. Struktura pracy  

### 2. Podstawy teoretyczne i analiza literatury

2.1. Architektura mikroserwisowa  
2.2. Observability w systemach rozproszonych  
2.3. Logi aplikacyjne jako źródło danych diagnostycznych  
2.4. Narzędzia i standardy observability  
2.5. Duże modele językowe jako interfejs analityczny  
2.6. Model Context Protocol jako standard integracji LLM z narzędziami  
2.7. Bezpieczeństwo MCP i aplikacji agentowych  
2.8. Porównanie MCP z alternatywnymi podejściami integracyjnymi  
2.9. Wnioski dla projektu własnego  

### 3. Analiza problemu i wymagań

3.1. Charakterystyka problemu analizy logów w mikroserwisach  
3.2. Scenariusze diagnostyczne  
3.3. Wymagania funkcjonalne  
3.4. Wymagania niefunkcjonalne  
3.5. Analiza ryzyka  
3.6. Przypadki użycia  

### 4. Projekt systemu

4.1. Architektura projektowanego rozwiązania  
4.2. Model danych logów  
4.3. Projekt narzędzi MCP  
4.4. Projekt zasobów i szablonów promptów MCP  
4.5. Mechanizmy bezpieczeństwa i ograniczenia dostępu  
4.6. Wybór technologii  

### 5. Implementacja

5.1. Struktura projektu  
5.2. Konfiguracja serwera MCP  
5.3. Implementacja narzędzi do wyszukiwania i agregacji logów  
5.4. Implementacja narzędzi do analizy trace'ów i kontekstu błędów  
5.5. Walidacja danych wejściowych i schematy odpowiedzi  
5.6. Obsługa błędów  
5.7. Maskowanie danych wrażliwych i audyt wywołań  
5.8. Testy jednostkowe i integracyjne  

### 6. Ewaluacja rozwiązania

6.1. Opis środowiska testowego  
6.2. Metodyka ewaluacji  
6.3. Scenariusze testowe  
6.4. Metryki ilościowe  
6.5. Ocena jakościowa odpowiedzi  
6.6. Analiza bezpieczeństwa  
6.7. Analiza wydajności  
6.8. Dyskusja wyników  

### 7. Zakończenie

7.1. Podsumowanie zrealizowanych celów  
7.2. Odpowiedzi na pytania badawcze  
7.3. Ograniczenia pracy  
7.4. Możliwości dalszego rozwoju  

### Bibliografia

### Wykaz rysunków

### Wykaz tabel

### Załączniki

## Planowany rezultat pracy

Rezultatem pracy będzie prototyp serwera MCP do analizy logów w systemach mikroserwisowych, opis jego projektu i implementacji, zestaw scenariuszy testowych oraz ocena przydatności rozwiązania w kontekście wspomaganej analizy incydentów.
