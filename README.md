# Power BI

## Definicje i skróty

- PI - PowerBI
- PQ - PowerQuery
- M - język programowania PowerQuery
- DAX - język formuł PowerBI

## Ogólne

- dane: transakcyjne, handlowcy, produkt
- pobieranie danych w Narzędzia główne - pobierz dane
- do źródeł dostajemy się przez menu: Plik -> (na dole) Opcje i ustawienia
- obsługa milionów wierszy
- direct query - pobieranie danych na żywo (raz na 15 minut)

## Wczytywanie źródła danych

### PowerQuery

- pokazuje tylko pierwsze rekordy
- jeżeli są ewidentne błędy - PQ proponuje na dole opcje Sugerowane tabele
- wybranie przekształć dane przenosi nas do PowerQuery
- po otwarciu PQ widzimy tabele - w widoku możemy włączyć opcję "Jakość kolumn" 
- jakość danych oceniana jest na podstawie 1000 wierszy
- próbkę 1000 wierszy możemy zmienić na wszystko, klikając w profilowanie w status bar PQ
- podczas przygotowania danych bardzo ważne są typy kolumn - "lewa góra"
- typ danych "abc 123" oznacza, że nie wybrano typu danych
- wszędzie w PQ wielkość znaków ma znaczenie
- PQ - pod prawym przyciskiem myszy jest użyteczna funkcja "Przekształć" (np. Małe na duże litery)
- dorzucenie prefiksu "tb_" (tabela danych) w nazwie tabeli pozwala na łatwiejsze wyszukanie
- ustawienia zapytania w PQ po prawej stronie, pokazują kroki jakie zostały zastosowane
- w narzędziach głównych PQ mamy funkcję usuwania pierwszych wierszy
- w menu przekształć mamy - użyj pierwszego wiersza jako nagłówka
- Zamknij i zastosuj realizuje przygotowane wcześniej czynności i ładuje dane
- warto mieć włączony pasek formuły (widok/Pasek formuły)
- można też wejść: Widok/Edytor zaawansowany
- aby przejść w czasie zwykłej pracy do PQ: narzędzia główne/"Przekształć dane"	
- istnieje możliwość podczas przekształcenia wygenerowania wierszy z jednej kolumny,
  w której dane odseparowane są jakimś znakiem (np. ",")
- możemy pivotować wybraną kolumnę, czyli wartości koluymnny stają się nowymi kolumnami,
  (należ określić, żeby nie agregować) - uruchomienie: "Kolumna przestawna"
- dostępne grupowanie działa analogicznie do grupowania w sql
- można wczytywać na raz wiele plików/table Excela stosując maski
- możliwe jest pobieranie plików z folderów
- kod w PQ edytuje się w języku "M"
- opcja scalania pozwala na łączenie zapytań (tabel) w jedną

### PowerBI - stan po wczytaniu

- po prawe stronie widać wszystkie wybrane tabele
- po lewej stronie są ikony:  
  - widok raportu
  - widok tabeli (podgląd danych, tylko do odczytu)
  - widok modelu (tabele z powiązaniami), wymagane do relacji i filtrowania
    (dane muszą być tego samego typu, jedna tabela unikalne wartości)
  - widok zapytań w modelu DAX
- można stworzyć dodatkowe tabele pomocnicze w języku DAX
- wiele do wielu rzadko się używa, raczej tabela pośrednia "bridge table" (słownikowa)
- w Narzędzia główne (widok raportu), mamy opcję "Wprowadź dane", pozwala na wprowadzenie tabeli z ręki
- można tworzyć w ten sposób pomocnicze tabele, np.: "_Obliczenia"
- tablicy pomocniczej możemy prawym klawiszem myszy utworzyć "nową miarę" (analogowa do formuły w Excelu)
- wpisując formułę miary, trzymając CTRL + rolka można zmieniać rozmiar
- po dodaniu miary do tabeli pomocniczej, możliwe jest usunięcie domyślnej pierwszej kolumny
- jeżeli chcemy po czymś filtrować to musi to być fizyczne pole w tabeli
- możliwe jest zaciąganie danych z internetu (url z csv)
- do edycji formuł w PB używamy języka "DAX" (w PQ jest "M"), DAX jest analogią do formuł Excela
 
#### DAX

- operuje na modelu danych (po załadowaniu danych)
- w celu rozpoczęci apracy z DAX
  - wchodzimy w widok tabeli, potem Narzędziagłowne/Wprowadź dane i zapisujemy
  - dodajemy nową miarę i usuwamy zbędną kolumnę (Kolumna 1)
  - możemy też dodać "nową kolumnę" do istniejhącej tabeli
-  operatory logincze: lub - "||", i "&&"
-  funkcja related() pozwala na odwołanie się w formule danej tabeli do innej tabeli w celu pobrania wartości
-  calculate -  bardzo ważna funkcja, jeżeli podamy drugi argiment filtru, to będzie on niezależny od fragmentatora
   tego samego pola
 - podczas podawania warunku, ciąg stringu podajemy w podwójnym cudzysłowie
 - 

#### Widok modelu (ralacji)

- w tym miejscy tworzymy lub oglądamy relacje między tabelami
- rodzaje tabel:
  -   Lookup tables (nazywane: wymiarów/słownikowe/):
    - krótkie (mało danych)
    - musi zawierać pole unikalne
    - dane rzadko  się zmieniają
  - Data Tables - tabele transakcyjne (tabele faktu)
    - dużo danych
    - często zmienne
    - nie musi zawierać unikatów
- definiując relacje można określić kierunek - obie strony (najczęściej używa się jeden)
- kierunek przeciągania podczas tworzenia relacji nie ma znaczenia
- PB wiąże domyślnie tabele po zgodnych nazwa kolumn (+ wykrywanie krotności)
- w praktyce tworzy się daty z tabeli kalendarza z polami dat tabel transakcyjnych
- tylko jedna relacja daty może być aktywna (pozostałe mają przerywaną linię)
- koleś tabele z danymi ustawia na dole, a słownikowe wyżej
- ustawione relacje mają bezpośredni wpływ dla prezentacje wizualizacji, ponieważ
  określają filtrowanie danych - kierunek filtrowania wskazaju strzałka w diagramie
  Należy na to uważać, gdy coś się źle liczy
- gość poleca klucze obce w tabelach ukrywać (kropeczki), żeby użytkownik przez przypadek tego nie użył
  (fofnięcie przez pokaż ukryte)

#### Miary
- koleś poleca dla id ustawiać string a nie int (uniknięcie liczenia)
- miara liczona jest dla tebeli i nie pojawia się w widoku danych tylko na drzewie tabeli
  w postaci ikony "kalkulatora"
- wartość miary wyliczana jest w kontekście filtra
- tworząc miary możemy używać miar zdefionwanych już wcześniej (wpływa to na lepszą wydajność)
- najpierw tworzymy atomowe miary, a kolejne miary niech z nich korzystają

  
#### Tworzenie wizualizacji

- klikamy pusty obszar raportu, następnie ikonę wizualizacji, np. kartę
- możemy też przeciągnąć pole na sekcję "Kolumny" w pasku "Wizualizacje", sekcję tą za pomocą grota 
  (w tym miejscu możemy również zmienić nazwę/etykietę kontrolki)
- następnie na grafikę karty możemy przeciągnąć pole, np.: sumę lub zaznaczać checkboxy tabel
możemy rozwijać i wybrać np. średnią. max albo inne wyliczenie
- możemy też przeciągnąć wartość wielokrotnie na pole "Etykietki" i dla każdej z nich wybrać inną rolę (max,min,suma) - pozwoli to na prezentację tych danych w "chmurce"
- przeciągając pole tabeli na legendę wizualizacji otrzymamy różne kolory
- jeżeli chcemy dostosować wizualizację, używamy drugiej ikony - Sformatuj element i wizualny 
- "mapa drzewa" (prostokąty jest fajna ponieważ mieści się w małych miejscach)
- mapę drzewa ogląda się z od lewego górnego rogu (rzeczy najbardziej istotne)
- domyślnie wszystkie komponenty wizualizacji są interaktywne - zależne od siebie
- klawiszem kontrol można użyć multiselekcji elementów
- PB domyślnie dla pól typu data dodaje automatycznie dodatkowe pole hierarchii - należy to wyłączyć 
  i nie korzystać (Plik->Ustawienia->Opcje->Analiza czasowa/Automatyczna..) - to jest amatorszczyzna
- jeżeli chcemy dodatkowo filtrować dane - można użyć komponentu "Fragmentator" lub "Fragmentator przycisku"
- przy polu data i użycia fragmentatora pojawia się pasek zakresu
- w komponentach wizualizacji jest Karta z wieloma wirszami

#### Tabela kalendarza (dat)

- taka tabela musi mieć rekord na każdy dzień
- naprojściej zrobić to z modelowania/nowa tabela/tdb_Daty = CALENDARAUTO()
  (funkcja wygeneruje w tabeli rekordy na każdy dzień na podstawie zakresu dat z danych)
- funkcja generuje zawsze dane od początku do końca roku
- w przypadku pobrania nowych danych, tabela ta automatycznie się aktualizuje na ich podstawie
- możemy nazwy miesięcy wygenerować słownie, ale wówczas może być problem z sortowaniem,
  w takiej sytuacji dodajemy dodatkowo kolumnę liczbową z nr miesiąca, następnie wybieramy
  Kolumnę tekstową, następnie sortuj wg kolumny - wskazujemy numeryczną 
- tydzień nie jest sprawą banalną, problem z ustaleniem pierwszego tygodnia.
  - amerykański - kiedy występuje 1 stycznia lub 1 tydzień gdy pierwszy stycznia min. W czwartek,
  dodatkowo ważne kiedy jest pierwszy dzień tygodnia (niedziela czy poniedziałek)
- wrzucenie na komponent wizualizacji roku powoduje jego sumowanie, ponieważ jest 
  wartością liczbową, żeby temu zaradzić wchodzimy Narzędzia kolumn i polu "Podsumowanie"
  wybieramy wartość "nie sumuj"
- zasada: wszystkie kolumny z datami z danych powinny być połączone z naszą tabelą kalendarza
- tworzenie hierarchii: zaczynamy klikając prawy myszy na polu najbardziej ogólnym
  tabeli kalendarza (np. rok)/Utwórz hierarchię, następnie dodajemy do hierarchii kolejne elementy
  np. Kwartał, miesiąc, tydzień itp. Następnie na wizualizacji dodajemy hierarchię co umożliwia
  Sterowanie zagnieżdżeniem za pomocą ikon.
- Strzałki równoległe pozwalają na porównywanie okresów do siebie, np. Miesięcy czy kwartałów
  
#### Tooltip

- pozwala na wyświetlenie dowolnej przygotowanej wcześniej strony (zakładki),
  po najechaniu myszą na element wizualizacji (np. Punkt wykresu)
- w celu włączeniu, dodajemy nową stronę, wchodzimy w ikonę formatowanie/Informacje o stronie
  następnie "Zezwalaj na używanie tej strony jako etykiety narzędzia"
  potem wracamy do naszej wizualizacji i komponentu:, Ikona sformatuj element/Ogólne/Etykietki/Strona
- funkcjonalność pozwala na niezaśmiecanie raportu
- taką stronę toolstipa należy ukryć przez użytkownikiem (prawy/ukryj)

#### Odświeżanie danych

- Narzędzia główne/"Przekształć dane"/V "Ustawienia źródła danych"/"Zmień źródło"
- następnie jeszcze należy kliknąć "Odśwież"
- odświeżenie danych stosuje kroki zdefiniowane w PQ podczas przekształcania danych

#### Dostosowywanie motywu

- nie powiększać pojedynczy elementów przez formatowanie, tylko użyć opcji: Widok/Dostosuj motyw

### Złote myśli prowadzącego

- jeżeli mamy jakikolwiek atrybut związany z tym kiedy coś się stało (np. Miesiąc, okres, rok)
  powinniśmy go zawsze przekonwenterować do daty
- następnie możemy dodać pole z rokiem: Rok = YEAR(tdb_Daty[Date]) 
- następnie kwartałem (w nazwie z "Q"), 

### Przekształcanie danych

#### Excel

- tabela przestawna (pivot), to tabela służąca do analizy danych, idzie na szerokość,
  kolumny to np. kolejne lata
- tabela nie pivot zawiera surowe dane, źródłowe - idzie na długość
- można zaznaczyć komórki w poziomie i wkleić specjalnie (wartości + transpozycja),
  pozwoli to na "obrócenie danych"
- celu odpivotowania zestawu danych zaznaczamy kolumny "dobrze zapisane" i z menu kontekstowego
  wybieramy opcję: "Anuluj przestawienie innych kolumn" 
- w arkusz Excela można zaznaczyć kilka komórek i utworzyć z nich tabelę ad-hoc
  (Wstawianie/Tabela), można wprowadzić jej nazwę na zakładce "Projekt tabeli"

### Uwagi

- daty pełnią ważną rolę (można poprawiać przez PowerQuery)
- PowerBI Desktop jest za darmo
- opcje w PB dzielą się na globalne i bieżącego pliku
- filtry pozwalają na filtrowanie danych dla bieżącej strony lub wszystkich stron
- w Narzędzia główne można dołączać zapytanie (dodać dane z jednej tabeli do drugiej)
- fajnym wykresem jest wykres kaskadowy, pozwala na znalezienie przyczyny
- wykresy można pobrać dodatkowo z sieci
- Gartner robi wykresy leaderów
