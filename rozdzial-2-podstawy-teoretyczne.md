# 2. Podstawy teoretyczne i analiza literatury

Wersja robocza, pierwsze podejście.

## 2.1. Architektura mikroserwisowa

Architektura mikroserwisowa jest jednym z dominujących podejść do projektowania współczesnych systemów rozproszonych. Jej podstawowym założeniem jest dekompozycja aplikacji na zestaw mniejszych, niezależnych usług, z których każda odpowiada za wybrany obszar funkcjonalny systemu. W przeciwieństwie do aplikacji monolitycznych, w których większość logiki biznesowej jest wdrażana jako jedna całość, mikroserwisy są uruchamiane jako odrębne procesy i komunikują się ze sobą przez mechanizmy sieciowe, takie jak HTTP, kolejki komunikatów lub inne protokoły wymiany wiadomości. Fowler i Lewis wskazują, że usługi tego typu są zwykle budowane wokół zdolności biznesowych oraz mogą być niezależnie wdrażane i skalowane.[^fowler-microservices]

Z technicznego punktu widzenia mikroserwis można traktować jako spójny, niezależny proces komunikujący się z innymi komponentami za pomocą wiadomości. Taką definicję przyjmują Dragoni i współautorzy, podkreślając niezależność usługi oraz jej ograniczony zakres odpowiedzialności.[^dragoni-microservices] W praktyce oznacza to, że każda usługa może mieć własny cykl życia, własny model danych, osobny zespół utrzymaniowy oraz technologię dobraną do konkretnego problemu. Ten model daje dużą elastyczność organizacyjną i technologiczną, ale przenosi część złożoności z wnętrza aplikacji do komunikacji między usługami.

Najważniejsze korzyści mikroserwisów wynikają z niezależnego wdrażania, możliwości selektywnego skalowania oraz lepszego dopasowania granic systemu do domen biznesowych. Newman zwraca uwagę, że przejście od monolitu do mniejszych autonomicznych usług pozwala sprawniej zarządzać dużym systemem, ale jednocześnie wprowadza problemy typowe dla systemów rozproszonych: trudniejszą komunikację, testowanie, monitorowanie, bezpieczeństwo i odporność na awarie.[^newman-building-microservices]

Z perspektywy niniejszej pracy szczególnie istotne jest to, że awaria w systemie mikroserwisowym rzadko ogranicza się do jednego wpisu logu lub jednego procesu. Żądanie użytkownika może przechodzić przez kilka usług, a błąd widoczny w jednej z nich może być skutkiem problemu powstałego wcześniej w innej usłudze. Analiza incydentu wymaga więc odtworzenia ścieżki wykonania żądania, powiązania logów z różnych komponentów i zrozumienia zależności między nimi. Właśnie ta właściwość systemów mikroserwisowych uzasadnia potrzebę narzędzi, które nie tylko wyszukują logi, ale również pomagają korelować je w kontekście całego przepływu.

## 2.2. Observability w systemach rozproszonych

Pojęcie observability odnosi się do zdolności wnioskowania o stanie wewnętrznym systemu na podstawie danych emitowanych na zewnątrz. W systemach informatycznych są to przede wszystkim logi, metryki i ślady rozproszone. OpenTelemetry opisuje observability jako możliwość zrozumienia systemu z zewnątrz, szczególnie w sytuacjach, których wcześniej nie przewidziano i dla których nie istnieje gotowy dashboard lub alert.[^otel-observability]

Monitoring i observability są ze sobą powiązane, ale nie są pojęciami równoważnymi. Monitoring odpowiada zwykle na pytanie, czy znany wcześniej parametr przekroczył określony próg. Observability ma szerszy zakres: pozwala zadawać nowe pytania o zachowanie systemu, analizować nietypowe przypadki i rekonstruować przyczyny zdarzeń. W systemach mikroserwisowych jest to szczególnie ważne, ponieważ liczba możliwych kombinacji stanów, opóźnień i błędów między usługami jest bardzo duża.

Tradycyjnie wyróżnia się trzy podstawowe sygnały telemetryczne: logi, metryki i trace'y. Logi są zapisem zdarzeń, często w formie komunikatów tekstowych lub strukturalnych rekordów. Metryki są liczbowym opisem zachowania systemu w czasie, na przykład liczbą żądań, czasem odpowiedzi lub odsetkiem błędów. Trace'y opisują przepływ pojedynczego żądania przez wiele komponentów systemu. Każdy z tych sygnałów odpowiada na inny typ pytania diagnostycznego. Logi pomagają ustalić, co dokładnie się wydarzyło. Metryki pokazują skalę i dynamikę zjawiska. Trace'y pozwalają prześledzić przebieg żądania w systemie rozproszonym.

W praktyce największą wartość diagnostyczną daje korelacja sygnałów. Przykładowo sama metryka wzrostu błędów HTTP 500 wskazuje na problem, ale nie wyjaśnia jego przyczyny. Trace może pokazać, w której usłudze wystąpiło opóźnienie lub wyjątek. Logi pozwalają następnie przeanalizować szczegóły błędu. OpenTelemetry wskazuje trzy podstawowe wymiary korelacji logów z innymi danymi telemetrycznymi: czas wykonania, kontekst wykonania, czyli między innymi `trace_id` i `span_id`, oraz kontekst zasobu, opisujący pochodzenie danych, na przykład usługę, kontener lub środowisko.[^otel-logs]

W pracy przyjęto, że logi nie powinny być analizowane jako izolowane komunikaty. Powinny być traktowane jako część szerszego modelu observability, w którym znaczenie wpisu zależy od czasu, usługi, poziomu błędu, identyfikatora śladu i metadanych środowiskowych. Takie założenie jest kluczowe dla projektu serwera MCP, ponieważ narzędzia udostępniane modelowi językowemu muszą zwracać dane w sposób możliwy do interpretacji i korelacji.

## 2.3. Logi aplikacyjne jako źródło danych diagnostycznych

Logi aplikacyjne są jednym z najstarszych i najczęściej wykorzystywanych źródeł informacji diagnostycznej. Ich znaczenie nie zmniejszyło się mimo rozwoju metryk i trace'ów, ponieważ logi często zawierają szczegółowy opis wyjątków, decyzji biznesowych, parametrów operacji i stanu aplikacji w konkretnym momencie. W systemie rozproszonym logi są jednak użyteczne tylko wtedy, gdy można je wyszukać, filtrować i połączyć z innymi zdarzeniami.

Istotnym rozróżnieniem jest podział na logi tekstowe i logi strukturalne. Log tekstowy jest zwykle komunikatem przeznaczonym dla człowieka. Może być łatwy do przeczytania, ale trudniejszy do automatycznej analizy, ponieważ informacja o czasie, usłudze, identyfikatorze żądania lub typie błędu może być ukryta w niespójnym formacie. Log strukturalny zapisuje dane jako rekord z polami, na przykład w formacie JSON. Dzięki temu system logowania, narzędzie analityczne lub serwer MCP może precyzyjnie odwoływać się do pól takich jak `timestamp`, `service`, `level`, `message`, `trace_id`, `span_id`, `http.status_code` czy `deployment.environment`.

OpenTelemetry podkreśla znaczenie spójnych modeli danych i semantycznych konwencji nazw. Konwencje semantyczne umożliwiają stosowanie tych samych nazw atrybutów w różnych usługach, językach programowania i backendach observability.[^otel-semconv] Dla tej pracy ma to bezpośrednie znaczenie projektowe: im bardziej spójne są logi wejściowe, tym łatwiej zaprojektować narzędzia MCP zwracające poprawne i przewidywalne wyniki.

Jednym z problemów systemów logowania jest kardynalność pól indeksowanych. W Grafana Loki etykiety służą przede wszystkim do selekcji strumieni logów, ale dokumentacja projektu ostrzega przed używaniem wartości dynamicznych, takich jak identyfikatory żądań, identyfikatory użytkowników lub trace ID, jako etykiet indeksowanych, ponieważ może to prowadzić do bardzo dużej liczby strumieni i pogorszenia wydajności.[^loki-labels] Wniosek projektowy jest następujący: identyfikatory takie jak `trace_id` są niezbędne do korelacji, ale sposób ich przechowywania i wyszukiwania musi uwzględniać charakter backendu logów.

Logi mogą również zawierać dane wrażliwe: adresy e-mail, tokeny, dane osobowe, identyfikatory klientów albo fragmenty zapytań z parametrami. W klasycznym interfejsie administracyjnym ryzyko ujawnienia takich danych dotyczy głównie użytkowników mających dostęp do systemu logowania. W przypadku integracji z LLM pojawia się dodatkowy etap przetwarzania: dane z logów mogą trafić do kontekstu modelu, do klienta MCP lub do zewnętrznego serwera. Dlatego projekt serwera MCP musi obejmować maskowanie danych, ograniczanie zakresu wyników oraz audyt wywołań narzędzi.

## 2.4. Narzędzia i standardy observability

Współczesne środowiska mikroserwisowe korzystają z wielu narzędzi do zbierania, przechowywania i analizowania danych telemetrycznych. Do najczęściej spotykanych należą stos ELK/Elastic, Grafana Loki oraz OpenTelemetry. Każde z tych podejść rozwiązuje nieco inny problem i przyjmuje inne założenia dotyczące modelu danych.

Elasticsearch i stos ELK są często wykorzystywane do pełnotekstowego indeksowania i wyszukiwania logów. Ich zaletą jest elastyczność zapytań i możliwość analizy dużych zbiorów dokumentów. W kontekście pracy Elasticsearch można traktować jako przykład backendu, który dobrze wspiera wyszukiwanie po wielu polach i analizę tekstu, ale wymaga zaprojektowania indeksów, mapowań oraz mechanizmów kontroli kosztu zapytań.

Grafana Loki przyjmuje inne założenie. Zamiast indeksować pełną treść każdego wpisu logu, Loki indeksuje przede wszystkim metadane w postaci etykiet, natomiast treść logów przechowuje w skompresowanych fragmentach.[^loki-docs] Takie podejście może obniżyć koszt przechowywania i uprościć eksploatację, ale wymaga ostrożnego projektowania etykiet. W przypadku analizy logów z użyciem LLM oznacza to, że narzędzie MCP powinno uwzględniać ograniczenia backendu i nie generować zapytań prowadzących do niepotrzebnie szerokiego skanowania danych.

OpenTelemetry nie jest pojedynczym backendem logów, lecz standardem i zestawem narzędzi do instrumentacji, zbierania i przesyłania danych telemetrycznych. Jego znaczenie polega na ujednoliceniu sposobu opisu logów, metryk, trace'ów i zasobów. Dzięki temu dane z różnych usług mogą zostać wzbogacone wspólnym kontekstem, na przykład informacją o kontenerze, podzie Kubernetes, nazwie usługi lub środowisku.[^otel-logs] Dla tej pracy OpenTelemetry jest ważne jako punkt odniesienia dla modelu danych logów oraz korelacji logów z trace'ami.

Wybór backendu logów wpływa na projekt narzędzi MCP. Jeżeli backend wspiera szybkie zapytania pełnotekstowe, narzędzie `search_logs` może oferować bogatsze filtrowanie po treści. Jeżeli backend jest zoptymalizowany pod etykiety i zakresy czasu, narzędzia powinny preferować filtrowanie po stabilnych metadanych. W każdym przypadku interfejs MCP powinien ukrywać część złożoności zapytań przed użytkownikiem, ale nie może ignorować ograniczeń technicznych systemu przechowywania logów.

## 2.5. Duże modele językowe jako interfejs analityczny

Duże modele językowe mogą pełnić rolę interfejsu pośredniego między użytkownikiem a specjalistycznym systemem technicznym. Ich zaletą jest możliwość przyjmowania zapytań w języku naturalnym, streszczania wyników, wskazywania hipotez i proponowania kolejnych kroków diagnostycznych. W kontekście analizy logów oznacza to, że użytkownik nie musi znać pełnej składni zapytań ani dokładnej struktury indeksu, aby rozpocząć eksplorację danych.

Sama zdolność modelu do generowania tekstu nie wystarcza jednak do rzetelnej analizy systemu operacyjnego. Model musi mieć dostęp do aktualnych danych oraz narzędzi, które pobiorą te dane z systemu źródłowego. W tym miejscu pojawia się pojęcie tool use lub function calling, czyli mechanizmu, w którym model wybiera odpowiednią funkcję, podaje argumenty, otrzymuje wynik i wykorzystuje go w odpowiedzi. Dokumentacja OpenAI dotycząca MCP i connectorów opisuje taki przepływ w kontekście zdalnych serwerów MCP: najpierw model otrzymuje listę narzędzi, a następnie może wykonać konkretne wywołanie narzędzia i włączyć wynik do kontekstu odpowiedzi.[^openai-mcp]

Zastosowanie LLM w analizie logów ma jednak istotne ograniczenia. Po pierwsze, model może błędnie zinterpretować niepełne lub źle dobrane dane. Po drugie, może przedstawić hipotezę jako bardziej pewną, niż wynika to z dostępnych logów. Po trzecie, może zostać podatny na treści znajdujące się w samych logach, jeżeli zawierają one instrukcje lub dane spreparowane przez atakującego. Po czwarte, użytkownik może nadmiernie zaufać odpowiedzi modelu i pominąć niezależną weryfikację.

Dlatego w niniejszej pracy LLM nie jest traktowany jako autonomiczny administrator systemu, lecz jako interfejs wspierający analizę. Odpowiedzialność za wiarygodność danych pozostaje po stronie narzędzi MCP, które powinny zwracać ograniczone, sprawdzalne i dobrze opisane wyniki. Odpowiedzialność decyzyjna pozostaje po stronie użytkownika, który powinien widzieć, jakie narzędzia zostały wywołane, z jakimi parametrami i na jakim zakresie danych.

## 2.6. Model Context Protocol

Model Context Protocol jest otwartym protokołem służącym do łączenia aplikacji wykorzystujących modele językowe z zewnętrznymi źródłami danych i narzędziami. Specyfikacja MCP opisuje trzy główne role: host, client i server. Hostem jest aplikacja użytkownika, na przykład klient czatu lub środowisko programistyczne. Client jest komponentem utrzymującym połączenie z serwerem MCP. Server udostępnia kontekst, zasoby i narzędzia, które mogą zostać wykorzystane przez model.[^mcp-spec]

MCP używa komunikatów JSON-RPC, co pozwala opisać żądania, odpowiedzi, błędy i powiadomienia w ustandaryzowany sposób.[^mcp-spec] Serwer MCP może udostępniać trzy podstawowe typy funkcjonalności: zasoby, prompty i narzędzia. Zasoby reprezentują dane, które klient lub model może odczytać. Prompty są szablonami interakcji lub przepływów pracy. Narzędzia są funkcjami, które model może wywołać, aby wykonać określoną operację, na przykład wyszukać logi, pobrać szczegóły trace'a albo przygotować agregację błędów.

W aktualnej specyfikacji ważne miejsce zajmują również funkcje oferowane przez klienta serwerowi: sampling, roots i elicitation.[^mcp-spec] Sampling umożliwia serwerowi inicjowanie żądań do modelu za pośrednictwem klienta. Roots pozwalają klientowi przekazać granice kontekstu, w którym serwer powinien operować. Elicitation umożliwia serwerowi uzyskanie dodatkowych informacji od użytkownika. W niniejszej pracy najważniejszy jest jednak kierunek serwerowy: projekt narzędzi i zasobów udostępniających logi klientowi LLM.

MCP definiuje dwa standardowe mechanizmy transportu: `stdio` oraz Streamable HTTP.[^mcp-transports] Transport `stdio` jest użyteczny dla lokalnych integracji, w których klient uruchamia serwer MCP jako proces potomny. Jest to prosty model dla prototypu, ponieważ nie wymaga wystawiania serwera do sieci. Streamable HTTP jest przeznaczony dla scenariuszy zdalnych i produkcyjnych, gdzie serwer działa jako osobny proces dostępny przez HTTP. Ten wariant wymaga większej uwagi w zakresie uwierzytelniania, nagłówków Origin, zarządzania sesją i bezpieczeństwa.

Z punktu widzenia pracy szczególnie ważne są schematy wejścia i wyjścia narzędzi. Specyfikacja narzędzi MCP przewiduje definiowanie `inputSchema`, a także możliwość dostarczania schematu wyjściowego dla wyników strukturalnych.[^mcp-tools] Dla analizy logów ma to znaczenie praktyczne: model językowy powinien wiedzieć, jakie parametry może przekazać, a klient powinien otrzymać wynik w przewidywalnym formacie. Przykładowo narzędzie `search_logs` powinno ograniczać zakres czasu, liczbę wyników i dozwolone filtry. Narzędzie `get_trace` powinno wymagać poprawnego `trace_id`. Narzędzie `error_summary` powinno zwracać nie tylko opis tekstowy, lecz także dane liczbowe umożliwiające weryfikację.

Rozwój MCP obejmuje również ekosystem rejestrów i serwerów. Oficjalny MCP Registry ma ułatwiać odkrywanie publicznie dostępnych serwerów MCP i standaryzować sposób dystrybucji informacji o nich.[^mcp-registry] Dla pracy ma to znaczenie głównie kontekstowe: pokazuje, że MCP rozwija się jako standard interoperacyjności, ale jednocześnie wprowadza pytania o zaufanie do serwerów, jakość metadanych i bezpieczeństwo narzędzi.

## 2.7. Bezpieczeństwo MCP i aplikacji agentowych

Integracja LLM z narzędziami i danymi operacyjnymi zwiększa możliwości systemu, ale jednocześnie rozszerza powierzchnię ataku. W klasycznej aplikacji użytkownik lub system wykonuje jawne zapytanie do API. W aplikacji agentowej model może sam wybrać narzędzie, przygotować argumenty i zinterpretować wynik. Taki model działania wymaga szczególnej kontroli nad zakresem danych i skutkami wywołań.

Specyfikacja MCP podkreśla znaczenie zgody użytkownika, prywatności danych i ostrożności przy wywoływaniu narzędzi.[^mcp-spec] W przypadku zdalnych serwerów MCP dochodzi również problem autoryzacji. Specyfikacja autoryzacji MCP bazuje na wybranych mechanizmach OAuth 2.1, OAuth Protected Resource Metadata i mechanizmach odkrywania serwera autoryzacji.[^mcp-auth] Dla prototypu analizującego logi wystarczający może być prostszy model lokalny, ale rozdział projektowy powinien omówić, jak rozwiązanie musiałoby zostać rozszerzone w środowisku produkcyjnym.

Jednym z najważniejszych zagrożeń dla aplikacji LLM jest prompt injection. W przypadku analizy logów problem ten ma szczególną postać: treść logu może pochodzić od użytkownika końcowego, z nagłówka HTTP, parametru formularza lub komunikatu błędu. Atakujący może więc doprowadzić do zapisania w logu instrukcji skierowanej do modelu. Jeżeli taka treść zostanie później przekazana do LLM bez odpowiedniego oznaczenia jako dane nieufne, model może potraktować ją jako instrukcję. OWASP klasyfikuje prompt injection, ujawnianie danych wrażliwych, niebezpieczny projekt pluginów i nadmierną autonomię jako istotne klasy ryzyka w aplikacjach LLM.[^owasp-llm]

Drugim istotnym zagrożeniem jest tool poisoning. Polega ono na umieszczaniu złośliwych instrukcji w metadanych narzędzi, na przykład w ich opisach, nazwach parametrów lub treściach pomocniczych. Huang i współautorzy analizują to zagrożenie w kontekście klientów MCP i wskazują, że metadane narzędzi mogą wpływać na decyzje modelu dotyczące wyboru i użycia narzędzi.[^huang-mcp-security] Maloyan i Namiot wskazują natomiast na szersze problemy architektoniczne, między innymi brak silnej atestacji deklarowanych możliwości serwera i propagację zaufania w konfiguracjach wieloserwerowych.[^maloyan-mcp-security]

W kontekście niniejszej pracy podstawowe mechanizmy bezpieczeństwa powinny obejmować: walidację wejścia, ograniczanie zakresów czasowych, limit liczby wyników, maskowanie danych wrażliwych, klasyfikację narzędzi według ryzyka, audyt wywołań i wyraźne rozdzielenie danych od instrukcji. Szczególnie ważne jest, aby narzędzia MCP w prototypie miały charakter tylko do odczytu. Serwer analizujący logi nie powinien wykonywać działań naprawczych, modyfikować danych produkcyjnych ani uruchamiać dowolnych komend systemowych.

Bezpieczeństwo należy traktować nie jako dodatek implementacyjny, ale jako część modelu badawczego. Ewaluacja rozwiązania powinna więc obejmować nie tylko skuteczność znajdowania błędów, lecz także próby przekroczenia dozwolonego zakresu zapytania, próby wydobycia danych wrażliwych i przykłady logów zawierających złośliwe instrukcje.

## 2.8. MCP na tle alternatywnych podejść

MCP nie jest jedynym sposobem integracji LLM z systemem logowania. Alternatywą może być klasyczne REST API, specyfikacja OpenAPI użyta do function calling, system RAG lub tradycyjny dashboard observability. Każde z tych podejść ma inne zalety i ograniczenia.

REST API jest dobrze znanym i stabilnym sposobem udostępniania danych. Pozwala zbudować dedykowane endpointy, na przykład `/logs/search` lub `/traces/{id}`. Jego ograniczeniem w kontekście LLM jest to, że sam standard REST nie opisuje pełnego modelu interakcji z aplikacją AI. Potrzebne są dodatkowe schematy, dokumentacja, logika wyboru endpointów oraz kod integracyjny po stronie klienta.

OpenAPI i function calling pozwalają opisać funkcje dostępne dla modelu oraz ich parametry. Jest to podejście skuteczne w wielu aplikacjach, ale zwykle wiąże definicję narzędzi z konkretnym klientem lub konkretną platformą LLM. MCP próbuje rozwiązać ten problem przez standaryzację relacji między klientem AI a zewnętrznym serwerem narzędzi. Serwer MCP może teoretycznie zostać wykorzystany przez różne hosty i klientów bez przepisywania całej integracji.

RAG, czyli retrieval-augmented generation, polega na pobieraniu fragmentów danych i przekazywaniu ich do modelu jako kontekstu. W analizie logów może to oznaczać indeksowanie wpisów logów w bazie wektorowej i wyszukiwanie semantyczne. Takie podejście jest użyteczne przy dokumentacji, raportach lub nieustrukturyzowanych zbiorach tekstu, ale przy logach operacyjnych może być niewystarczające. Analiza incydentu często wymaga precyzyjnych filtrów po czasie, poziomie błędu, nazwie usługi i identyfikatorze śladu. MCP nie wyklucza RAG, ale w tej pracy jest traktowany jako bardziej bezpośrednia warstwa wywoływania kontrolowanych operacji diagnostycznych.

Klasyczne dashboardy observability, takie jak Kibana lub Grafana, dają dużą kontrolę ekspertowi. Ich zaletą jest transparentność zapytań, wizualizacja danych i możliwość dokładnej eksploracji. Ich ograniczeniem jest próg wejścia: użytkownik musi wiedzieć, gdzie szukać, jak formułować zapytania i jak interpretować wyniki. LLM połączony z MCP może obniżyć ten próg, ponieważ pozwala rozpocząć analizę od pytania w języku naturalnym. Nie zastępuje jednak dashboardów, lecz może pełnić rolę warstwy wspierającej, szczególnie na etapie wstępnej diagnozy.

Z tej perspektywy MCP można rozumieć jako protokół integracyjny, który nie konkuruje bezpośrednio z backendami observability. MCP nie przechowuje logów i nie zastępuje Elasticsearch, Loki ani OpenTelemetry. Jego rolą jest udostępnienie wybranych, kontrolowanych operacji analitycznych aplikacji LLM. Wartość takiego podejścia zależy od jakości zaprojektowanych narzędzi, spójności danych wejściowych, mechanizmów bezpieczeństwa oraz sposobu ewaluacji wyników.

## 2.9. Wnioski dla projektu własnego

Analiza literatury i dokumentacji prowadzi do kilku wniosków projektowych. Po pierwsze, serwer MCP do analizy logów powinien operować na logach strukturalnych, ponieważ tylko wtedy możliwe jest wiarygodne filtrowanie i korelowanie danych. Po drugie, identyfikatory `trace_id` i `span_id` powinny być traktowane jako podstawowy mechanizm łączenia wpisów pochodzących z różnych usług. Po trzecie, narzędzia MCP powinny mieć wąsko zdefiniowane odpowiedzialności, jawne schematy wejścia i wyjścia oraz ograniczenia chroniące backend przed kosztownymi zapytaniami.

Po czwarte, LLM powinien być traktowany jako pomocniczy interfejs analityczny, a nie jako samodzielne źródło prawdy. Odpowiedzi generowane przez model muszą być powiązane z danymi źródłowymi, które użytkownik może zweryfikować. Po piąte, bezpieczeństwo musi być uwzględnione już na poziomie wymagań i projektu narzędzi, ponieważ logi mogą zawierać zarówno dane wrażliwe, jak i treści nieufne.

W konsekwencji część projektowa pracy powinna skoncentrować się nie tylko na implementacji serwera MCP, lecz także na uzasadnieniu wyboru narzędzi, schematów danych, ograniczeń bezpieczeństwa i metody ewaluacji. Taki układ pozwoli połączyć praktyczny charakter projektu z wymaganym w pracy magisterskiej komponentem analitycznym.

---

## Przypisy robocze

[^fowler-microservices]: Martin Fowler, James Lewis, "Microservices", https://martinfowler.com/articles/microservices.html, dostęp: 2026-06-22.

[^dragoni-microservices]: Nicola Dragoni et al., "Microservices: yesterday, today, and tomorrow", arXiv:1606.04036, https://arxiv.org/pdf/1606.04036, dostęp: 2026-06-22.

[^newman-building-microservices]: Sam Newman, "Building Microservices, 2nd Edition", O'Reilly Media, 2021; opis wydawcy: https://www.oreilly.com/library/view/building-microservices-2nd/9781492034018/, dostęp: 2026-06-22.

[^otel-observability]: OpenTelemetry, "Observability primer", https://opentelemetry.io/docs/concepts/observability-primer/, dostęp: 2026-06-22.

[^otel-logs]: OpenTelemetry, "Logging", https://opentelemetry.io/docs/specs/otel/logs/, dostęp: 2026-06-22.

[^otel-semconv]: OpenTelemetry, "Semantic Conventions", https://opentelemetry.io/docs/concepts/semantic-conventions/, dostęp: 2026-06-22.

[^loki-labels]: Grafana Loki, "Label best practices", https://grafana.com/docs/loki/latest/get-started/labels/bp-labels/, dostęp: 2026-06-22.

[^loki-docs]: Grafana Loki, "Grafana Loki documentation", https://grafana.com/docs/loki/latest/, dostęp: 2026-06-22.

[^openai-mcp]: OpenAI, "MCP and Connectors", https://developers.openai.com/api/docs/guides/tools-connectors-mcp, dostęp: 2026-06-22.

[^mcp-spec]: Model Context Protocol, "Specification 2025-11-25", https://modelcontextprotocol.io/specification/2025-11-25, dostęp: 2026-06-22.

[^mcp-transports]: Model Context Protocol, "Transports", https://modelcontextprotocol.io/specification/2025-11-25/basic/transports, dostęp: 2026-06-22.

[^mcp-tools]: Model Context Protocol, "Tools", https://modelcontextprotocol.io/specification/2025-11-25/server/tools, dostęp: 2026-06-22.

[^mcp-registry]: Model Context Protocol Blog, "Introducing the MCP Registry", https://blog.modelcontextprotocol.io/posts/2025-09-08-mcp-registry-preview/, dostęp: 2026-06-22.

[^mcp-auth]: Model Context Protocol, "Authorization", https://modelcontextprotocol.io/specification/2025-11-25/basic/authorization, dostęp: 2026-06-22.

[^owasp-llm]: OWASP Foundation, "OWASP Top 10 for Large Language Model Applications", https://owasp.org/www-project-top-10-for-large-language-model-applications/, dostęp: 2026-06-22.

[^huang-mcp-security]: Charoes Huang, Xin Huang, Ngoc Phu Tran, Amin Milani Fard, "Model Context Protocol Threat Modeling and Analyzing Vulnerabilities to Prompt Injection with Tool Poisoning", arXiv:2603.22489, https://arxiv.org/abs/2603.22489, dostęp: 2026-06-22.

[^maloyan-mcp-security]: Narek Maloyan, Dmitry Namiot, "Breaking the Protocol: Security Analysis of the Model Context Protocol Specification and Prompt Injection Vulnerabilities in Tool-Integrated LLM Agents", arXiv:2601.17549, https://arxiv.org/abs/2601.17549, dostęp: 2026-06-22.
