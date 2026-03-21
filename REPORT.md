# Start
Na początku zrobiłem setup notebooka - zorientowałem się w bibliotekach matplot i numpy, napisałem podział danych, funkcje straty, skalowanie, etc.

Głównie proste funkcje bez logiki.
Przy następnym podejściu spróbuję zastosować rozwiązanie analityczne - opiera się głównie na mnożeniach macierzy, więc wszystko jest już zaimplementowane w numpy

# 20.03

### podstawowa konfiguracja
Po lepszym rozpisaniu funkcji testowałem prostą konfigurację:

podział zbioru 60/20/20->
standaryzacja jako skalowanie -> kwadratowa funkcja straty -> regresja grzbietowa z rozwiązaniem analitycznym

W pliku `Analytical_std.png` znajduje się wykres lambda/MSE z pierwszego uruchomienia działającego kodu.
Widać że dla małych lambd wynik się nie zmienia, a przeskok do większych lambd nie jest ogromny.
Na wykresie pominąłem `lambda=0` - okazało się, że błąd dla 0 jest ogromny i bardzo zaburza mi skalę wykresu. Mimo optymalnego rozwiązania błąd jest całkiem duży - dla walidacji wyszło `9829.01`, a na danych testowych: `11191.28`

Dla porównania na obrazku `abs_std.png` jest wykres z błędem absolutnym - mimo, że rozwiązanie analityczne tutaj nie działa, to wykres jest bardzo zbliżony kształtem do poprzedniego\
Dla innego skalowania również niewiele się zmienia - wykres na `analytical_mm_sqr.png`

Raczej wszystko tu działa - następnym zadaniem będzie zaimplementowanie gradientu.

# 21.03
Napisałem 3 wersje gradientu.

Napotkałem błąd, w którym wartość theta wystrzeliwała do NaN bardzo szybko.\

Po debugowaniu okazało się że dane nie są skalowane (byłem przekonany że są). Dowiedziałem się przy tym, że rozwiązanie analityczne działa poprawnie bez skalowania.

W gradiencie jako warunek stopu wykorzystałem liczbę iteracji - mogę obserwować jak zachowuje się wykres w zależności od l. iteracji, theta inicjalizuję zerami.

Szybki test - ile potrzeba iteracji (dla eta=0.0001), aby osiągnąć wynik zbliżony do idealnego:

Rozwiązanie analityczne w pliku `perfect_grad_1.png` - błąd na danych testowych: `10773.388`\
Wykres dla gradientu dla 10000 iteracji `10000_iter.png` - błąd: `10773.375`\
Dla 1000 iteracji błąd (w zaokrągleniu do 3 miejsc po przecinku) na danych testowych jest taki sam jak dla 10000, pominę wykres.\
Błąd dla 100 iteracji: `10773.378` - niewiele mniejszy, dane się dobrze ułożyły.\
Dla 10 iteracji (plik `10_iters.png`) błąd: `14493.027` - błąd znacznie większy, kształt wykresu taki sam, ale cały wykres podniósł się do góry.

Po kilku testach zauważyłem, że w moim przypadku granica w miarę dobrego wyniku to około 50.\
Wniosek z obserwacji: nie potrzeba ogromnej liczby iteracji żeby dostać dobre rozwiązanie - odchylenie od błędu z rozwiązania analitycznego maleje niemalże wykładniczo.

Po doświadczeniu zacząłem inicjalizować theta wartościami losowymi - nie wpłynęło to znacznie na wynik, ale obiektywnie losowa inicjalizacja jest lepsza więc ją zostawię.

Przekształciłem walidację na osobne funkcje i zacząłem wyznaczanie krzywej uczenia.\
Dla podstawowej konfiguracji (60/20/20, standaryzacja, błąd kwadratowy, regresja grzbietowa, gradient) krzywa uczenia znajduje się w pliku `default_curve.png`
