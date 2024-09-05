# Dissecting-PE-Headers

Hej dziś dowiemy się więcej o plikach wykonywalnych Portable Executable i jak działają ich nagłówki.
W tym pokoju dowiemy się nieco więcej o:
Zrozumienie różnych nagłówków w pliku PE
Nauka czytania nagłówków PE
Zidentyfikuj spakowane pliki wykonywalne
Wykorzystaj informacje z nagłówków PE do analizy złośliwego oprogramowania

W tym writeup będę głównie zamieszczał odpowiedźi na pytania w zadaniach, oraz jak doszedłem do takiego bądź inego rozwiązania.


<h3>1.Przegląd nagłówków PE</h3>

Task 1:Jakiego typu danych są nagłówki PE?
Wszystkie te nagłówki są typu danych  STRUCT


<h3>2.IMAGE_DOS_HEADER i DOS_STUB</h3>

Task 1:Ile bajtów znajduje się w IMAGE_DOS_HEADER?
IMAGE_DOS_HEADER składa się z pierwszych 64 bajtów pliku PE

Task 2:Co oznacza MZ?
Znaki MZ oznaczają inicjały Marka Zbikowskiego , jednego z architektów Microsoftu, który stworzył format pliku MS- DOS 

Task 3:W której zmiennej IMAGE_DOS_HEADER zapisany jest adres IMAGE_NT_HEADERS?
Odpowiedźią bedzie: e_lfanew

![image](https://github.com/user-attachments/assets/5336c527-ba9f-4106-b6ef-1d56ad4bde38)

Task 4:W załączonej maszynie wirtualnej otwórz plik PE Desktop/Samples/zmsuz3pinwl w drzewie PE. Jaki jest adres IMAGE_NT_HEADERS dla tego pliku PE?

![image](https://github.com/user-attachments/assets/b92a0267-7a9d-4df3-99cd-6390d41d2026)


<h3>3.NAGŁÓWKI_OBRAZU_NT</h3>

Task 1:W załączonej maszynie wirtualnej znajduje się plik Desktop\Samples\zmsuz3pinwl. Otwórz ten plik w pe-tree. Czy ten plik PE jest skompilowany dla maszyny 32-bitowej czy 64-bitowej?
W powyższym przykładzie widać, że architektura to i386, co oznacza, że ​​ten plik PE jest zgodny z 32-bitową architekturą Intel.
```0x0103 RELOCS_STRIPPED | EXECUTABLE_IMAGE | 32BIT_MACHINE```

Task 2:Jaki jest znacznik czasu i daty tego pliku?
0x62289d45 Wed Mar  9 12:27:49 2022 UTC

0x0103 RELOCS_STRIPPED | EXECUTABLE_IMAGE | 32BIT_MACHINE


<h3>4.OPCJONALNY_NAGŁÓWEK</h3>

Task 1:Która zmienna z OPTIONAL_HEADER wskazuje, czy plik jest aplikacją 32-bitową czy 64-bitową?
Magic:  Liczba Magic informuje, czy plik PE jest aplikacją 32-bitową czy 64-bitową

Task 2:Jaka wartość Magic wskazuje, że plik jest aplikacją 64-bitową?
0x020B, oznacza to aplikację 64-bitową

Task 3:Jaki jest podsystem pliku  Desktop\Samples\zmsuz3pinwl?
Możemy to sprawdzić bezpośrednio w subsystem: 0x0003 WINDOWS_CUI

![image](https://github.com/user-attachments/assets/1dee546c-2427-476a-ba3c-8225e503a523)


<h3>5.NAGŁÓWEK_SEKCJI_OBRAZU</h3>

Task 1:Ile sekcji ma plik Desktop\Samples\zmsuz3pinwl?
Możemy to w łatwy sposób policzyć, jest to 7:

![image](https://github.com/user-attachments/assets/bc1d5a8b-2167-4ad9-9009-c701e2bd4ce2)


Task 2:Jakie są cechy sekcji .rsrc pliku  Desktop\Samples\zmsuz3pinwl
Możemy to podejrzeć przechodząc do zakładki .rsrc

![image](https://github.com/user-attachments/assets/8ac11e45-c2a1-43bb-9e59-f5fca9cb0df7)


<h3>6.OPIS_IMPORTU_OBRAZU</h3>

Task 1:Plik PE Desktop\Samples\redline importuje funkcję CreateWindowExW. Z którego pliku dll importuje tę funkcję?
Już tłumaczę, przejdźmy sobie do (rozwiń zakładkę) User32.dll, zjedźając coraz to niżej możesz zobaczyć: 

![image](https://github.com/user-attachments/assets/1c6d07c4-cece-43d1-9790-efbd5cf3f5d1)


<h3>7.Pakowanie i identyfikacja spakowanych plików wykonywalnych</h3>

Task 1:Który z plików na dołączonej maszynie wirtualnej w katalogu Desktop\Samples wydaje się być spakowanym plikiem wykonywalnym?
Odpowiedźią jest: zmsuz3pinwl, ponieważ:

Nietypowe nazwy sekcji
Uprawnienia EXECUTE dla wielu sekcji
Wysoka entropia, zbliżająca się do 8, w niektórych sekcjach.
Istotna różnica między SizeOfRawData i Misc_VirtualSize niektórych sekcji PE
Bardzo mało funkcji importu
