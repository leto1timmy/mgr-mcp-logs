# Log zmian i rejestr źródeł

Ten plik służy do śledzenia dwóch rzeczy:

- jakie zmiany wprowadzamy w planie i późniejszej treści pracy;
- z jakich źródeł korzystamy oraz do których rozdziałów można je przypisać.

Zasada robocza: każda większa decyzja redakcyjna, projektowa lub bibliograficzna powinna mieć wpis w logu zmian. Każde źródło powinno mieć stabilny identyfikator, aby można było później łatwo przenieść je do przypisów i bibliografii.

---

## Konwencja identyfikatorów

| Typ wpisu | Format | Przykład |
| --------- | ------ | -------- |
| Zmiana | `CHG-RRRR-MM-DD-NN` | `CHG-2026-06-22-01` |
| Źródło MCP | `MCP-...` | `MCP-SPEC-2025-11-25` |
| Źródło OpenTelemetry | `OTEL-...` | `OTEL-LOGS` |
| Źródło bezpieczeństwa | `SEC-...` | `SEC-OWASP-LLM-TOP10` |
| Źródło naukowe | `PAPER-...` | `PAPER-HUANG-MCP-2026` |

---

## Log zmian

| ID zmiany | Data | Plik / sekcja | Co zmieniono | Dlaczego | Powiązane źródła |
| --------- | ---- | ------------- | ------------ | -------- | ---------------- |
| `CHG-2026-06-22-01` | 2026-06-22 | `praca-magisterska-plan.md` | Przebudowano plan z opisu implementacji na plan pracy badawczo-projektowej. Dodano problem badawczy, cele szczegółowe, pytania badawcze, zakres, rozszerzoną teorię, analizę ryzyk, ewaluację i metryki. | Komentarz promotora wskazywał potrzebę większej ilości treści analitycznych i teoretycznych. Nowa struktura wyraźnie oddziela teorię, analizę problemu, projekt, implementację i ewaluację. | `MCP-SPEC-2025-11-25`, `MCP-CHANGELOG-2025-11-25`, `OTEL-LOGS`, `SEC-OWASP-LLM-TOP10`, `PAPER-HUANG-MCP-2026`, `PAPER-MALOYAN-MCP-2026` |
| `CHG-2026-06-22-02` | 2026-06-22 | `praca-magisterska-plan.md` / temat | Doprecyzowano temat: dodano "ewaluację" i "wspomaganą przez LLM analizę logów". | Temat powinien zapowiadać nie tylko implementację serwera, ale także ocenę jego przydatności i rolę LLM jako interfejsu analitycznego. | `MCP-SPEC-2025-11-25`, `OPENAI-MCP-CONNECTORS` |
| `CHG-2026-06-22-03` | 2026-06-22 | `praca-magisterska-plan.md` / rozdział 2 | Rozbudowano podstawy teoretyczne o observability, logi strukturalne, OpenTelemetry, architekturę MCP, bezpieczeństwo MCP i porównanie z alternatywami. | Rozdział 2 ma być główną odpowiedzią na uwagę promotora o większej zawartości teoretycznej. | `OTEL-OBSERVABILITY`, `OTEL-LOGS`, `OTEL-SEMCONV`, `MCP-SPEC-2025-11-25`, `MCP-TRANSPORTS-2025-11-25`, `MCP-AUTH-2025-11-25`, `MCP-TOOLS-2025-11-25` |
| `CHG-2026-06-22-04` | 2026-06-22 | `praca-magisterska-plan.md` / rozdział 6 | Zmieniono "Prezentację rozwiązania" na "Ewaluację i analizę wyników". Dodano metryki ilościowe, ocenę jakościową, analizę bezpieczeństwa i dyskusję wyników. | Praca magisterska powinna zawierać ocenę rozwiązania, a nie tylko demonstrację działania. | `OPENAI-MCP-CONNECTORS`, `SEC-OWASP-LLM-TOP10`, `PAPER-HUANG-MCP-2026`, `PAPER-MALOYAN-MCP-2026` |
| `CHG-2026-06-22-05` | 2026-06-22 | `praca-magisterska-plan.md` / narzędzia MCP | Rozszerzono listę narzędzi o `top_error_patterns`, `compare_time_windows`, `explain_error_context` i `audit_tool_usage`. | Nowe narzędzia lepiej wspierają część analityczną, porównywanie scenariuszy i audyt bezpieczeństwa. | `MCP-TOOLS-2025-11-25`, `OPENAI-MCP-CONNECTORS`, `MCP-SECURITY-BEST-PRACTICES` |
| `CHG-2026-06-22-06` | 2026-06-22 | `praca-magisterska-log-zmian-i-zrodla.md` | Utworzono osobny plik do logowania zmian i śledzenia źródeł. | Praca będzie wymagała przypisów i bibliografii, więc źródła muszą być śledzone od początku, razem z decyzjami projektowymi i redakcyjnymi. | Wszystkie źródła z rejestru |
| `CHG-2026-06-22-07` | 2026-06-22 | `praca-magisterska-log-zmian-i-zrodla.md` / rejestr źródeł | Zweryfikowano i poprawiono tytuł źródła `PAPER-MCP-SURVEY-2025`; oznaczono je jako preprint nierecenzowany. | Rejestr źródeł powinien przechowywać dokładne dane bibliograficzne i rozróżniać dokumentację, raporty oraz literaturę recenzowaną lub nierecenzowaną. | `PAPER-MCP-SURVEY-2025` |
| `CHG-2026-06-22-08` | 2026-06-22 | `praca-magisterska-plan-skrocony.md` | Utworzono skróconą wersję planu pracy przeznaczoną do udostępnienia promotorowi. | Pełny plan roboczy jest zbyt szczegółowy do szybkiej konsultacji. Skrócony plan pokazuje temat, cel, problem badawczy, pytania, strukturę i rezultat pracy. | `MCP-SPEC-2025-11-25`, `OTEL-OBSERVABILITY`, `MCP-TOOLS-2025-11-25` |
| `CHG-2026-06-22-09` | 2026-06-22 | `rozdzial-2-podstawy-teoretyczne.md` | Napisano pierwszą roboczą wersję rozdziału 2: mikroserwisy, observability, logi, narzędzia observability, LLM jako interfejs, MCP, bezpieczeństwo i porównanie podejść. | Rozdział 2 ma bezpośrednio zwiększyć zawartość teoretyczno-analityczną pracy i stworzyć podstawę dla późniejszego projektu oraz ewaluacji. | `FOWLER-MICROSERVICES`, `PAPER-DRAGONI-MICROSERVICES-2017`, `NEWMAN-BUILDING-MICROSERVICES-2021`, `OTEL-OBSERVABILITY`, `OTEL-LOGS`, `OTEL-SEMCONV`, `GRAFANA-LOKI`, `GRAFANA-LOKI-LABELS`, `MCP-SPEC-2025-11-25`, `MCP-TRANSPORTS-2025-11-25`, `MCP-TOOLS-2025-11-25`, `MCP-AUTH-2025-11-25`, `OPENAI-MCP-CONNECTORS`, `SEC-OWASP-LLM-TOP10`, `PAPER-HUANG-MCP-2026`, `PAPER-MALOYAN-MCP-2026` |
| `CHG-2026-06-22-10` | 2026-06-22 | `praca-magisterska-log-zmian-i-zrodla.md` / rejestr źródeł | Dodano źródła bazowe dla mikroserwisów, SRE i observability engineering. | Rozdział teoretyczny nie powinien bazować wyłącznie na dokumentacji technicznej MCP. Potrzebne są też źródła ogólne i naukowe do architektury mikroserwisowej oraz niezawodności. | `FOWLER-MICROSERVICES`, `PAPER-DRAGONI-MICROSERVICES-2017`, `NEWMAN-BUILDING-MICROSERVICES-2021`, `GOOGLE-SRE-SLO`, `MAJORS-OBSERVABILITY-ENGINEERING-2022` |

---

## Rejestr źródeł

| ID źródła | Typ | Tytuł / opis | Link | Dostęp | Do użycia w rozdziałach | Notatka robocza |
| --------- | --- | ------------ | ---- | ------ | ----------------------- | --------------- |
| `MCP-SPEC-2025-11-25` | specyfikacja techniczna | Model Context Protocol Specification, wersja 2025-11-25 | https://modelcontextprotocol.io/specification/2025-11-25 | 2026-06-22 | 2.6, 4.1, 4.3, 5.2 | Podstawowe źródło do opisu architektury MCP: host, client, server, JSON-RPC, capabilities, tools, resources, prompts, sampling, roots, elicitation. |
| `MCP-CHANGELOG-2025-11-25` | specyfikacja / changelog | Key Changes MCP 2025-11-25 | https://modelcontextprotocol.io/specification/2025-11-25/changelog | 2026-06-22 | 2.6, 2.7, 4.1, 7 | Źródło do pokazania aktualnego rozwoju protokołu: authorization discovery, icons, incremental scopes, elicitation, sampling with tools, tasks, JSON Schema 2020-12. |
| `MCP-TRANSPORTS-2025-11-25` | specyfikacja techniczna | MCP Transports | https://modelcontextprotocol.io/specification/2025-11-25/basic/transports | 2026-06-22 | 2.6, 4.1, 5.2 | Źródło do porównania transportu `stdio` i Streamable HTTP oraz do omówienia bezpieczeństwa Origin header, localhost binding i sesji. |
| `MCP-AUTH-2025-11-25` | specyfikacja techniczna | MCP Authorization | https://modelcontextprotocol.io/specification/2025-11-25/basic/authorization | 2026-06-22 | 2.7, 3.5, 4.6 | Źródło do opisu OAuth 2.1, protected resource metadata, OIDC discovery, scope selection i modelu ról autoryzacji. |
| `MCP-TOOLS-2025-11-25` | specyfikacja techniczna | MCP Tools | https://modelcontextprotocol.io/specification/2025-11-25/server/tools | 2026-06-22 | 2.6, 4.3, 5.3, 5.5, 5.6 | Źródło do projektowania narzędzi, schematów wejścia/wyjścia, structured content, walidacji i rozróżnienia błędów protokołu od błędów wykonania narzędzia. |
| `MCP-SECURITY-BEST-PRACTICES` | dokumentacja bezpieczeństwa | MCP Security Best Practices | https://modelcontextprotocol.io/docs/tutorials/security/security_best_practices | 2026-06-22 | 2.7, 3.5, 4.6, 6.5 | Źródło do opisania confused deputy, token passthrough, SSRF, session hijacking, local MCP compromise i scope minimization. |
| `OPENAI-MCP-CONNECTORS` | dokumentacja techniczna | OpenAI API: MCP and Connectors | https://developers.openai.com/api/docs/guides/tools-connectors-mcp | 2026-06-22 | 2.5, 2.6, 2.7, 4.6, 6.3 | Źródło do omówienia zdalnych serwerów MCP, connectorów, approvals, `mcp_list_tools`, `mcp_call`, `allowed_tools`, kosztu tokenów i ryzyk udostępniania danych serwerom MCP. |
| `MCP-REGISTRY-2025-09-08` | wpis projektowy | Introducing the MCP Registry | https://blog.modelcontextprotocol.io/posts/2025-09-08-mcp-registry-preview/ | 2026-06-22 | 2.6, 2.8, 7 | Źródło do opisu rejestru MCP, odkrywania serwerów, subrejestrów publicznych/prywatnych i problemu zaufania do serwerów. |
| `MCP-ANNIVERSARY-2025-11-25` | wpis projektowy | One Year of MCP: November 2025 Spec Release | https://blog.modelcontextprotocol.io/posts/2025-11-25-first-mcp-anniversary/ | 2026-06-22 | 2.6, 2.7, 7 | Źródło do kontekstu rozwoju MCP: governance, tasks, simplified authorization, extensions, enterprise features. |
| `ANTHROPIC-MCP-INTRO-2024` | wpis firmowy | Introducing the Model Context Protocol | https://www.anthropic.com/news/model-context-protocol | 2026-06-22 | 1, 2.6 | Źródło do genezy MCP i problemu fragmentacji integracji z danymi i narzędziami. |
| `ANTHROPIC-AAIF-2025` | wpis firmowy / governance | Donating the Model Context Protocol and establishing the Agentic AI Foundation | https://www.anthropic.com/news/donating-the-model-context-protocol-and-establishing-of-the-agentic-ai-foundation | 2026-06-22 | 2.6, 7 | Źródło do opisu dojrzałości ekosystemu, adopcji MCP i przekazania projektu pod Linux Foundation / AAIF. |
| `FOWLER-MICROSERVICES` | artykuł branżowy | Martin Fowler, James Lewis: Microservices | https://martinfowler.com/articles/microservices.html | 2026-06-22 | 2.1 | Źródło klasycznej definicji stylu mikroserwisowego: małe usługi, niezależne procesy, lekkie mechanizmy komunikacji, organizacja wokół zdolności biznesowych. |
| `PAPER-DRAGONI-MICROSERVICES-2017` | artykuł naukowy / rozdział | Microservices: yesterday, today, and tomorrow | https://arxiv.org/pdf/1606.04036 | 2026-06-22 | 2.1 | Źródło akademickie do definicji mikroserwisu jako spójnego, niezależnego procesu komunikującego się wiadomościami oraz do porównania z monolitem. |
| `NEWMAN-BUILDING-MICROSERVICES-2021` | książka | Sam Newman: Building Microservices, 2nd Edition | https://www.oreilly.com/library/view/building-microservices-2nd/9781492034018/ | 2026-06-22 | 2.1, 2.2, 4.7 | Źródło książkowe do projektowania, zarządzania, skalowania, testowania, monitorowania i zabezpieczania architektur mikroserwisowych. |
| `GOOGLE-SRE-SLO` | książka online / SRE | Google SRE Book: Service Level Objectives | https://sre.google/sre-book/service-level-objectives/ | 2026-06-22 | 2.2, 6.2, 6.3 | Źródło do pojęć SLI, SLO, dostępności i pomiaru niezawodności usług. |
| `MAJORS-OBSERVABILITY-ENGINEERING-2022` | książka | Charity Majors, Liz Fong-Jones, George Miranda: Observability Engineering | https://www.oreilly.com/library/view/observability-engineering/9781492076438/ | 2026-06-22 | 2.2, 2.3, 6.4 | Źródło książkowe do observability jako praktyki rozumienia złożonych systemów produkcyjnych i debugowania z użyciem bogatego kontekstu. |
| `OTEL-OBSERVABILITY` | dokumentacja techniczna | OpenTelemetry Observability Primer | https://opentelemetry.io/docs/concepts/observability-primer/ | 2026-06-22 | 2.2, 2.4 | Źródło do definicji observability, telemetrii, logów, metryk, trace'ów, SLI i SLO. |
| `OTEL-LOGS` | specyfikacja techniczna | OpenTelemetry Logging | https://opentelemetry.io/docs/specs/otel/logs/ | 2026-06-22 | 2.2, 2.3, 4.2 | Źródło do korelacji logów z trace'ami przez czas, trace context i resource context. |
| `OTEL-SEMCONV` | dokumentacja techniczna | OpenTelemetry Semantic Conventions | https://opentelemetry.io/docs/concepts/semantic-conventions/ | 2026-06-22 | 2.3, 4.2 | Źródło do ustandaryzowanych nazw atrybutów dla logów, metryk, trace'ów i zasobów. |
| `GRAFANA-LOKI` | dokumentacja techniczna | Grafana Loki Documentation | https://grafana.com/docs/loki/latest/ | 2026-06-22 | 2.4, 2.8 | Źródło do opisu Loki jako systemu indeksującego metadane/etykiety zamiast pełnej treści logów. |
| `GRAFANA-LOKI-LABELS` | dokumentacja techniczna | Grafana Loki Label Best Practices | https://grafana.com/docs/loki/latest/get-started/labels/bp-labels/ | 2026-06-22 | 2.3, 2.4, 4.2 | Źródło do omówienia kardynalności etykiet i problemu używania dynamicznych wartości, takich jak request ID lub trace ID, jako indeksowanych labeli. |
| `SEC-OWASP-LLM-TOP10` | standard / lista ryzyk | OWASP Top 10 for Large Language Model Applications | https://owasp.org/www-project-top-10-for-large-language-model-applications/ | 2026-06-22 | 2.7, 3.5, 6.5 | Źródło do ryzyk LLM: prompt injection, sensitive information disclosure, insecure plugin design, excessive agency, overreliance. |
| `SEC-NSA-MCP-2026` | raport bezpieczeństwa | NSA: Model Context Protocol Security Design Considerations for AI-Driven Automation | https://www.nsa.gov/Portals/75/documents/Cybersecurity/CSI_MCP_SECURITY.pdf | 2026-06-22 | 2.7, 3.5, 4.6, 6.5 | Źródło do tezy, że rozwój bezpieczeństwa MCP nie nadąża za adopcją oraz do praktycznych rekomendacji dla środowisk produkcyjnych. |
| `PAPER-HUANG-MCP-2026` | artykuł naukowy / preprint | Model Context Protocol Threat Modeling and Analyzing Vulnerabilities to Prompt Injection with Tool Poisoning | https://arxiv.org/abs/2603.22489 | 2026-06-22 | 2.7, 3.5, 6.5 | Źródło do STRIDE/DREAD, tool poisoning, porównania klientów MCP i wielowarstwowej obrony. |
| `PAPER-MALOYAN-MCP-2026` | artykuł naukowy / preprint | Breaking the Protocol: Security Analysis of the Model Context Protocol Specification and Prompt Injection Vulnerabilities in Tool-Integrated LLM Agents | https://arxiv.org/abs/2601.17549 | 2026-06-22 | 2.7, 3.5, 6.5 | Źródło do podatności protokołu: brak attestation możliwości, sampling bez origin authentication, implicit trust propagation i propozycja MCPSec. |
| `PAPER-MCP-SURVEY-2025` | artykuł naukowy / preprint | A Survey of the Model Context Protocol (MCP): Standardizing Context to Enhance Large Language Models (LLMs) | https://www.preprints.org/manuscript/202504.0245 | 2026-06-22 | 2.6, 2.8 | Źródło przeglądowe do architektury MCP, dynamic tool discovery i bezpieczeństwa w kontekście fragmentacji integracji. Preprint nie jest recenzowany. |
| `PAPER-AGENT-PROTOCOLS-2025` | artykuł naukowy / przegląd | A Survey of Agent Interoperability Protocols: MCP, ACP, A2A, ANP | https://arxiv.org/html/2505.02279v2 | 2026-06-22 | 2.6, 2.8, 7 | Źródło do szerszego kontekstu interoperacyjności agentów i porównania MCP z innymi protokołami agentowymi. |

---

## Mapowanie źródeł na rozdziały

| Rozdział | Główne źródła |
| -------- | ------------- |
| 1. Wstęp | `ANTHROPIC-MCP-INTRO-2024`, `MCP-ANNIVERSARY-2025-11-25`, `OTEL-OBSERVABILITY` |
| 2.1-2.4 Mikroserwisy i observability | `FOWLER-MICROSERVICES`, `PAPER-DRAGONI-MICROSERVICES-2017`, `NEWMAN-BUILDING-MICROSERVICES-2021`, `GOOGLE-SRE-SLO`, `MAJORS-OBSERVABILITY-ENGINEERING-2022`, `OTEL-OBSERVABILITY`, `OTEL-LOGS`, `OTEL-SEMCONV`, `GRAFANA-LOKI`, `GRAFANA-LOKI-LABELS` |
| 2.5 LLM jako interfejs analityczny | `OPENAI-MCP-CONNECTORS`, `SEC-OWASP-LLM-TOP10` |
| 2.6 MCP | `MCP-SPEC-2025-11-25`, `MCP-CHANGELOG-2025-11-25`, `MCP-TRANSPORTS-2025-11-25`, `MCP-TOOLS-2025-11-25`, `MCP-REGISTRY-2025-09-08`, `MCP-ANNIVERSARY-2025-11-25` |
| 2.7 Bezpieczeństwo | `MCP-AUTH-2025-11-25`, `MCP-SECURITY-BEST-PRACTICES`, `SEC-OWASP-LLM-TOP10`, `SEC-NSA-MCP-2026`, `PAPER-HUANG-MCP-2026`, `PAPER-MALOYAN-MCP-2026` |
| 2.8 Porównanie podejść | `MCP-SPEC-2025-11-25`, `OPENAI-MCP-CONNECTORS`, `PAPER-MCP-SURVEY-2025`, `PAPER-AGENT-PROTOCOLS-2025`, `GRAFANA-LOKI` |
| 3. Analiza problemu i wymagań | `OTEL-LOGS`, `SEC-OWASP-LLM-TOP10`, `PAPER-HUANG-MCP-2026` |
| 4. Projekt systemu | `MCP-SPEC-2025-11-25`, `MCP-TRANSPORTS-2025-11-25`, `MCP-AUTH-2025-11-25`, `MCP-TOOLS-2025-11-25`, `OTEL-LOGS` |
| 5. Implementacja | `MCP-TOOLS-2025-11-25`, `MCP-SECURITY-BEST-PRACTICES`, `OPENAI-MCP-CONNECTORS` |
| 6. Ewaluacja | `OPENAI-MCP-CONNECTORS`, `SEC-OWASP-LLM-TOP10`, `PAPER-HUANG-MCP-2026`, `PAPER-MALOYAN-MCP-2026` |
| 7. Podsumowanie | źródła z rozdziałów 2, 4 i 6 |

---

## Źródła do uzupełnienia

Te obszary wymagają jeszcze dalszego poszerzenia literatury naukowej lub książkowej:

- mikroserwisy: dodatkowe źródła recenzowane dotyczące wzorców awarii i eksploatacji systemów rozproszonych;
- observability i SRE: SLI/SLO, incident response, distributed tracing;
- analiza logów: log mining, anomaly detection, log parsing;
- LLM tool use i human-in-the-loop;
- metodologia ewaluacji narzędzi diagnostycznych.

---

## Otwarte decyzje

| ID | Decyzja | Status | Uwagi |
| -- | ------- | ------ | ----- |
| `DEC-001` | Czy implementować tylko `stdio`, czy także Streamable HTTP? | otwarte | Plan zakłada `stdio` jako wariant implementowany i Streamable HTTP jako wariant analityczny lub rozszerzenie. Decyzja zależy od czasu i wymagań promotora. |
| `DEC-002` | Czy backendiem prototypu zostaje PostgreSQL `jsonb`? | wstępnie tak | PostgreSQL upraszcza powtarzalność prototypu. Elasticsearch/Loki mogą zostać opisane porównawczo. |
| `DEC-003` | Czy testować z jednym klientem MCP czy kilkoma? | otwarte | Minimum: jeden klient MCP. Warto opisać różnice Claude Desktop / Cursor / OpenAI remote MCP, jeśli czas pozwoli. |
| `DEC-004` | Czy używać prawdziwego LLM w ewaluacji, czy symulować wywołania narzędzi? | otwarte | Najlepiej połączyć testy deterministyczne narzędzi z jakościową oceną przykładowych rozmów. |

---

## Szablon kolejnego wpisu w logu zmian

```text
| `CHG-RRRR-MM-DD-NN` | RRRR-MM-DD | plik / sekcja | Co zmieniono | Dlaczego | `SOURCE-ID-1`, `SOURCE-ID-2` |
```

## Szablon kolejnego wpisu źródła

```text
| `SOURCE-ID` | typ | tytuł / opis | link | data dostępu | rozdziały | notatka robocza |
```
