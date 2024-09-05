# Dissecting-PE-Headers

Hej dziś dowiemy się więcej o plikach wykonywalnych Portable Executable i jak działają ich nagłówki.
W tym pokoju dowiemy się nieco więcej o:
Zrozumienie różnych nagłówków w pliku PE
Nauka czytania nagłówków PE
Zidentyfikuj spakowane pliki wykonywalne
Wykorzystaj informacje z nagłówków PE do analizy złośliwego oprogramowania

W tym writeup będę głównie zamieszczał odpowiedźi na pytania w zadaniach, oraz jak doszedłem do takiego bądź inego rozwiązania.


<br>1.Przegląd nagłówków PE</br>

Task 1:Jakiego typu danych są nagłówki PE?
Wszystkie te nagłówki są typu danych  STRUCT


<br>2.IMAGE_DOS_HEADER i DOS_STUB</br>

Task 1:Ile bajtów znajduje się w IMAGE_DOS_HEADER?
IMAGE_DOS_HEADER składa się z pierwszych 64 bajtów pliku PE

Task 2:Co oznacza MZ?
Znaki MZ oznaczają inicjały Marka Zbikowskiego , jednego z architektów Microsoftu, który stworzył format pliku MS- DOS 

Task 3:W której zmiennej IMAGE_DOS_HEADER zapisany jest adres IMAGE_NT_HEADERS?
Odpowiedźią bedzie: e_lfanew

![image](https://github.com/user-attachments/assets/5336c527-ba9f-4106-b6ef-1d56ad4bde38)

Task 4:W załączonej maszynie wirtualnej otwórz plik PE Desktop/Samples/zmsuz3pinwl w drzewie PE. Jaki jest adres IMAGE_NT_HEADERS dla tego pliku PE?

![image](https://github.com/user-attachments/assets/b92a0267-7a9d-4df3-99cd-6390d41d2026)


<br>3.NAGŁÓWKI_OBRAZU_NT</br>

Task 1:W załączonej maszynie wirtualnej znajduje się plik Desktop\Samples\zmsuz3pinwl. Otwórz ten plik w pe-tree. Czy ten plik PE jest skompilowany dla maszyny 32-bitowej czy 64-bitowej?
W powyższym przykładzie widać, że architektura to i386, co oznacza, że ​​ten plik PE jest zgodny z 32-bitową architekturą Intel.
```0x0103 RELOCS_STRIPPED | EXECUTABLE_IMAGE | 32BIT_MACHINE```

Task 2:Jaki jest znacznik czasu i daty tego pliku?
0x62289d45 Wed Mar  9 12:27:49 2022 UTC

0x0103 RELOCS_STRIPPED | EXECUTABLE_IMAGE | 32BIT_MACHINE


<br>4.OPCJONALNY_NAGŁÓWEK</br>

Task 1:Która zmienna z OPTIONAL_HEADER wskazuje, czy plik jest aplikacją 32-bitową czy 64-bitową?
Magic:  Liczba Magic informuje, czy plik PE jest aplikacją 32-bitową czy 64-bitową

Task 2:Jaka wartość Magic wskazuje, że plik jest aplikacją 64-bitową?
0x020B, oznacza to aplikację 64-bitową

Task 3:Jaki jest podsystem pliku  Desktop\Samples\zmsuz3pinwl?
Możemy to sprawdzić bezpośrednio w subsystem: 0x0003 WINDOWS_CUI

![image](https://github.com/user-attachments/assets/1dee546c-2427-476a-ba3c-8225e503a523)


<br>5.NAGŁÓWEK_SEKCJI_OBRAZU</br>


