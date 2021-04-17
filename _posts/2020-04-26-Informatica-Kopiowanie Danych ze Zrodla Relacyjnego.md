---
layout: post
author: mleczkom
title:  Informatica PowerCenter Designer - Kopiowanie Danych ze Zrodla Relacyjnego
---

Dzisiaj chciałbym Wam pokazać jak z wykorzystaniem Informatica PowerCenter przekopiować dane z jednej tabeli relacyjnej do drugiej tabeli relacyjnej.
Całość implementacji rozwiązania będzie składała się z 4 kroków:
1. Utworzenia źródła relacyjnego
2. Utworzenia celu relacyjnego
3. Utworzenie mapping-u
4. Utworzenie workflow-u 


# 1. Utworzenie zrodla relacyjnego

Mianowicie, możemy to zrobić na dwa sposoby:
* ręcznie, gdzie sami musimy zdefiniować metadane źródła (nazwy kolumny oraz typy danych),
* lub z wykorzystaniem jednej z funkcjonalności Designer-a, która automatycznie zaczyta metadane wskazanego przez nas źródła danych.

Bardziej praktyczne wydaje się skorzystanie z drugiej opcji i to na niej skupię się w tym wpisie.
Na początku uruchamiamy Designer-a, łączymy się z naszym ulubionym repozytorium a następnie przechodzimy do zakladki `Source Analyzer` [1]
![relconn0]({{ site.baseurl }}/images/RelationalConnection00.png)
  
a nastepnie uzywamy funkcji `Sources->Import from Database...` <BR>
![relconn1]({{ site.baseurl }}/images/RelationalConnection01.png)

Zmieniamy zrodlo klikajac `...` [1] a nastepnie w nowo otwartym oknie w zakladce `DNS uzytkownika` wybieramy istniejace juz zrodlo danych badz dodajemy nowe klikajac `Dodaj...` [2]. Ja pokaze jakie kroki musimy wykonac aby dodac nowe zrodlo. Klikamy `Dodaj...` <BR> W nowo otwartym oknie wybieramy rodzaj sterownika -> `SQL Server Native Client 11.0` (w naszym przypadku jest to wersja 11.0 ale moze byc uzyta kazda dostepna)
  
![relconn2]({{ site.baseurl }}/images/RelationalConnection02.png)

a nastepnie klikamy `Zakoncz`. W kolejnym oknie wpisujemy nazwe naszego zrodla danych [1] a nastepnie podajemy nazwe/adress IP serwera [2]

![relconn3]({{ site.baseurl }}/images/RelationalConnection03.png)

i klikamy `Dalej`. Nastepnie zmieniamy rodzaj autentykacji na **SQL Server Authentication** [1] i podajemy nazwe i haslo uzytkownika ktorego bedziemy wykorzystwac do polaczenia z baza danych [2].

![relconn4]({{ site.baseurl }}/images/RelationalConnection04.png)

i klikamy `Dalej`. W kolejnym oknie zmieniamy nazwe bazy danych na taka do ktorej chcemy sie polaczyc. W naszym przypadku jest to baza `NORTHWND`

![relconn5]({{ site.baseurl }}/images/RelationalConnection05.png)

i klikamy `Dalej` i `Zakoncz` na kolejny.

![relconn6]({{ site.baseurl }}/images/RelationalConnection06.png)

Po utworzeniu nowego zrodla danych mozemy przejsc do utworzenia polaczenia do tabeli zrodlowej. W tym celu wybieramy [1] nasze nowo utworzone zrodlo danych. Ponownie podajemy nazwe uzytkownika [2] i haslo [3] ktorego bedziemy uzywac do laczenia sie z baza danych a nastepnie klikamy `Connect` [4]. 

![relconn7]({{ site.baseurl }}/images/RelationalConnection07.png)

Zmieniamy ustawienia [5] tak aby widziec wszystkie dostepne dla naszego uzytkownika obiekty (tabel, widoki itd.) w bazie danych. Wybieramy ten ktory nas interesuje [6] i klikamy `OK`. Powinnismy zobaczyc nowe zrodlo w folderze `Sources`.

![relconn8]({{ site.baseurl }}/images/RelationalConnection08.png)



# 2. Utworzenie celu relacyjnego

Z konfiguracja polaczenia do tabeli docelowej postepujemy podobnie jak z konfiguracja polaczenia do tabeli zrodlowej. Roznica jest taka ze zamiast przejsc do zakladki `Source Analyzer` przechodzimy do zakladki `Target Designer` [1] a nastepnie uzywamy funkcji `Targets->Imports from Database...`

![relconn9]({{ site.baseurl }}/images/RelationalConnection09.png)

Nastepnie wybieramy [1] nasze zrodlo danych (lub tworzymy nowe tak jak zostalo to pokazane w kroku o tworzeniu polaczenia do tabeli zrodlowej, podajemy nazwe uzytkownika [2] i haslo [3] ktorego bedziemy uzywac do laczenia sie z baza danych a nastepnie klikamy `Connect` [4]. 

![relconn10]({{ site.baseurl }}/images/RelationalConnection10.png)

Zmieniamy ustawienia [5] tak aby widziec wszystkie dostepne dla naszego uzytkownika obiekty (tabel, widoki itd.) w bazie danych. Wybieramy ten ktory nas interesuje [6] i klikamy `OK`. Powinnismy zobaczyc nowe target w folderze `Targets`.

![relconn11]({{ site.baseurl }}/images/RelationalConnection11.png)



# 3. Utworzenie mapping-u

Przechodzimy do zakladki `Mapping Designer` [1] a nastepnie przeciagamy do obszaru roboczego tabele zrodlowa [2] i tabele docelowa [3] i przeciagajac porty wyjsciowe ze zrodla do portow wejsciowych targetu tworzymy mapowanie kolumn [4].

![crtmapping01]({{ site.baseurl }}/images/CreateMapping01.png)

Skonczona prace zapisujemy wybierajac menu `Repository->Save`.

![crtmapping02]({{ site.baseurl }}/images/CreateMapping02.png)



# 4. Utworzenie workflow-u

Workflow tworzymy w narzedziu `Informatica PowerCenter Workflow Manager` ktorego mozemy otworzyc klikajac [1]. Nastepnie przechodzimy do zakladki `Workflow Designer` [2].

Aby utworzyc nowy workflow uzywamy menu `Workflows->Create`, podajemy nazwe naszego workflowu [3] oraz wybieramy **Integration Service** w ramach ktorego bedzie uruchamiany nasz workflow [4]. 

![crtworkflow01]({{ site.baseurl }}/images/CreateWorkflow01.png)<BR>

Nastepnie w zakladce `Properties` ustawiamy nazwe pliku log-u naszego workflow-u [2] oraz nazwe katalogu w ktorym ma byc on zapisywany.

![crtworkflow02]({{ site.baseurl }}/images/CreateWorkflow02.png)<BR>
  
Przy konfiguracji nazwy pliku log-u uzylismy jednej z [wbudowanych zmiennych](../98-Build-in-Variables/Build-in-Variables.md) -> **$PMWorkflowRunId**. Jeżeli potrzebujemy utworzyc zmiena uzytkownika mozemy to zrobic w zakladce `Variables` 

![crtworkflow03]({{ site.baseurl }}/images/CreateWorkflow03.png)<BR>
  
My utworzymy sobie 2 zmienne, dla kazdej z nich musimy okreslic jej nazwe [1], typ [2] oraz to czy chcemy aby ostatnia jej wartosc byla zapisywana do repozytorium [3]:
 * $$wf_WorkflowID
 * $$wf_StartTime. 
 
 Beda one przechowywaly odpowiednio identyfikator uruchomienia naszego workflow-u oraz date i godzine tego uruchomienia. Pozniej przekazemy te zmiene do parametrow naszego mappingu.

OK mamy utworzony workflow mozemy rozpoczac dodawanie do niego task-ow. Uzywamy do tego celu menu `Task->Create...`. W pierwszej kolejnosci dodamy task przypisania wartosci do wczesniej zdefiniowanych zmiennych.

![crtworkflow04]({{ site.baseurl }}/images/CreateWorkflow04.png)<BR>

Wybieramy `Assigment` jak typ dodawanego task-u [2], wprowadzamy jego nazwe [3], klikamy `Create` i zamykamy okienko. Kolejnym krokiem jest skonfigurowanie utworzonego task-u. W tym celu klikamy PPM na wybranym tasku i klikamy `Edit...`

![crtworkflow06]({{ site.baseurl }}/images/CreateWorkflow06.png)<BR>

Aby w tasku **Assigment** moc przypisac wartosci zmiennych musimy te zmienne wpierw utworzyc w przeciwnym wypadku zobaczymy taki oto komunikat. 

![crtworkflow05]({{ site.baseurl }}/images/CreateWorkflow05.png)<BR>

My mielismy nasze zmienne zdefiniowane wczesniej wiec mozemy przejsc do przypisania im wartosci.
![crtworkflow07]({{ site.baseurl }}/images/CreateWorkflow07.png)<BR>

Po raz kolejny uzywamy tutaj zmiennej **$PMWorkflowRunId** oraz **WORKFLOWSTARTTIME** ktora rowniez jest [zmienna wbudowana](../98-Build-in-Variables/Build-in-Variables.md). Na zakonczenie klikamy `OK`.

Kolejnym etapem jest ustawienie odpowiednich nastepstw pomiedzy zadaniami. Do tego celu wykorzystujemy menu `Tasks->Link Task` albo korzystamy z paska narzedzi.
![crtworkflow08]({{ site.baseurl }}/images/CreateWorkflow08.png)<BR>
  
![crtworkflow09]({{ site.baseurl }}/images/CreateWorkflow09.png)<BR>
  
Dodajemy kolejne zadanie, tym razem typu `Session`.

![crtworkflow10]({{ site.baseurl }}/images/CreateWorkflow10.png)<BR>

i wybieramy ktory z naszych mapping-ow chcemy w ramach tej sesji uruchamiac [1] i klikamy OK [2]
![crtworkflow11]({{ site.baseurl }}/images/CreateWorkflow11.png)<BR>

a nastepnie definiujemy nastepstwo, uruchomienie naszego mappingu w sesji ma nastapic po przypisaniu zmiennym wartosci.
![crtworkflow12]({{ site.baseurl }}/images/CreateWorkflow12.png)<BR>

Ostatnim krokiem bedzie skonfigurowanie sesji. Ale zanim do tego przejdziemy upewnimy sie czy mamy utworzone odpowiednie polaczenia do bazy danych. Wybieramy menu `Connections->Relational...`. 

![crtworkflow19]({{ site.baseurl }}/images/CreateWorkflow19.png)<BR>
  
Jeżeli w okienku ktore nam sie otworzylo widzimy juz utworzone polaczenie do naszej bazy nie musimy nic robic. Klikamy `Close` [4] i przechodzimy do konfiguracji sesji.<BR>
Ja natomiast pokaze jak dodac takie polaczenie gdyby nie bylo jeszcze zdefiniowane. Zaczynamy od wybrania `New...` [1] nastepnie wybieramy rodzaj/typ [2] naszego polaczenia. My bedziemy sie laczyc z MSSQL wiec wybieramy **Microsoft SQL Server**. Nasz wybor zatwierdzamy klikajac `OK` [3].  
  
![crtworkflow20]({{ site.baseurl }}/images/CreateWorkflow20.png)<BR>
  
Przechodzimy do okienka w ktorym ustawiamy nazwe naszego polaczenia [1], podajemy kredentiale ktorych bedziemy uzywac do laczenia sie z baza danych: uzytkownika [2] i haslo [3]. Na koncu podajemy nazwe bazy danych [4] i serwera na ktorym dziala usluga MSSQL [5]. Poprawnosc wprowadzonych informacji zatwierdzamy kliknieciem `OK` [6].

![crtworkflow21]({{ site.baseurl }}/images/CreateWorkflow21.png)<BR>

Proces ten powtarzamy tyle razy ile potrzebujemy polaczen do roznych baz danych.<BR> 

Po utworzeniu polaczen mozemy przejsc do skonfigurowania sesji. W tym celu klikamy PPM na poprzednio utworzonej sesji i wybieramy menu `Edit...`
  
![crtworkflow13]({{ site.baseurl }}/images/CreateWorkflow13.png)<BR>

W zakladce `Properties` konfigurujemy nazwe pliku logu-u [1] naszej sesji, nazwe katalogu w ktorym ten log bedzie zapisywany [2] oraz wskazujemy jezeli jest taka koniecznosc tzw. `Parameter File` [3]. Jest to plik ktory jest uzywany do ustawienia wartosci zmiennych uzywanych podczas sesji.
Ostatnia bardzo przydatna opcja ktora nalezy ustawic w przypadku naszego scenariusza jest `Treat source row as` [4]. Opcja ta determinuje [sposob/wplyw jaki na tabele docelowa beda mialy rekordy do niej plynace](https://docs.informatica.com/data-integration/powercenter/10-4-0/workflow-basics-guide/sources/working-with-relational-sources/defining-the-treat-source-rows-as-property.html). My wybieramy **Data driven** poniewaz w mappingu z wykorzystaniem transformacji **Updade Strategy** okreslamy rozne dzialania w zaleznosci od tego czy dany rekord jest nowy lub zmieniony. 
  
![crtworkflow14]({{ site.baseurl }}/images/CreateWorkflow14.png)<BR>
  
Ok, teraz zajmiemy sie konfiguracja naszego mappingu. W tym celu przechodzimy do zakladki `Mapping`, `Transformations` [1] i zaczynamy od ustawienia szczegolow polaczenia dla transformacji **Source Qualifier** [4]. Dla polaczen relacyjnych jako typ procesu czytajacego dane [2] ustawiamy **Relational Reader** a nastepnie wybieramy nazwe polaczenia do bazy danych [3] ktorego bedziemy uzywac w procesie pobierania danych.

![crtworkflow15]({{ site.baseurl }}/images/CreateWorkflow15.png)<BR>

Kolejny krok to konfiguracja polaczenia do naszej tabeli docelowej. Przechodzimy do [4] i podobnie jak w przypadku **Source Qualifier** okreslamy typ procesu, tym razem jednak, zapisujacego dane [2] jako **Relational Writer** a nastepnie wybieramy nazwe polaczenia do bazy danych [3] ktorego bedziemy uzywac w procesie zapisywania danych.

![crtworkflow16]({{ site.baseurl }}/images/CreateWorkflow16.png)<BR>
  
W przypadku konfigurowania polaczenia do tabeli docelowej musimy dodatkowo zwrocic uwage na tzn. `Target load type` [5]. Poprzez odpowienie ustawienie tej opcji decydujemy czy operacje na tabeli beda wykonywane w trybie **BULK** czy tez **NORMAL**. Co wazne, samo ustawienie trybu **BULK** nie gwarantuje nam ze operacje beda wykonywane w tym trybie. Dodatkowo sterownik uzywany w ramach danego polaczenia musi wspierac taki tryb. Uzywany przez nas sterownik **SQL Server Native Client 11.0** takiego trybu nie wspiera, wiec ustawienie tej opcji nie ma dla nas znaczenia.
Co wazne, w trybie **BULK** mozemy tylko insertowac.<BR>

Na zakonczenie skonfigurujemy sposob przekazywania wartosci ze zmiennych workflow-u do parametrow mapping-u. Przechodzimy do zakladki `Components` i klikamy `presession_variable_assignment` [1].

![crtworkflow16]({{ site.baseurl }}/images/CreateWorkflow17.png)<BR>

Na kolejnym ekranie konfigurujemy ktore zmienne workflow-u [4] maja byc przypisane [3] do ktorych zmiennych mapping-u [2].

![crtworkflow16]({{ site.baseurl }}/images/CreateWorkflow18.png)<BR>

