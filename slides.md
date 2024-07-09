---
theme: default
class: 'text-center'
highlighter: shiki
lineNumbers: true
drawings:
  persist: false
css: unocss
---

<div class="text-4xl"> Wydajność WASM+Rust w porównaniu z językiem JavaScript w kontekście aplikacji przeglądarkowych</div>

<div class="abs-bl m-6 gap-2 text-left">
  inż. Kasper Seweryn
  <div class="text-sm block">promotor: dr inż. Marek Tabędzki</div>
</div>

---
layout: cover
---

# Cel pracy

Celem pracy jest porównanie wydajności języka Rust, skompilowanego do WebAssembly, z językiem JavaScript w środowiskach przeglądarkowych oraz odpowiedzenie na pytanie: „Czy Rust jest lepszym wyborem dla aplikacji przeglądarkowych niż JavaScript?”.

---

# Technologie

## JavaScript

Jest to język programowania kompilowany Just-In-Time, który pozwala na dodanie interaktywności do stron internetowych.

## Rust

Jest bezpiecznym i wydajnym językiem programowania, który dokonuje wykrywania błędów podczas kompilacji Ahead-Of-Time.

## WebAssembly

Jest to format, który pozwala na uruchomienie innych języków niż JavaScript w przeglądarce internetowej z prędkością zbliżoną do natywnej.

---

# Badania

Po dogłębnym przeanalizowaniu wszystkich technologii postanowiłem zbadać następujące aspekty:

- Re-enkodowanie stringów między kontekstem JavaScript, a WebAssembly
- Enkodowanie stringów do Base64
- Wyliczanie k-tej liczby Fibonacciego rekurencyjnie
- Mnożenie macierzy 4x4
- Wyliczanie sumy CRC-32
- Wyliczanie sumy CRC-64
- API DOM --- Tworzenie elementów
- API DOM --- Aktualizacja co drugiego elementu
- API DOM --- Usuwanie elementów

---

# Środowisko testowe
Wszystkie testy przeprowadziłem w następującym środowisku:

Komputer:
- MacBook Air z 2020 roku
  - procesor Apple M1
  - 16 GB pamięci RAM
  - system operacyjny macOS Sonoma 14.5

Przeglądarka:
- Google Chrome w wersji 126.0.6478.62

---

# Procedura testowania

- Każdy test składał się z zera lub więcej funkcji JavaScriptowych oraz jednej lub więcej funkcji napisanych w języku Rust.
- Każdy test został podzielony na 3 podtesty, które różniły się danymi wejściowymi.
- Dane wejściowe były losowo generowane przed rozpoczęciem pomiaru czasu wykonania funkcji.
- Każdy podtest był wykonany dla trzech różnych ilości powtórzeń oznaczonych symbolem $n$.
- Badane w każdym teście funkcje były wykonywane sekwencyjnie, a przed pierwszym wywołaniem każdej funkcji (dla każdego podtestu oraz każdego $n$), karta przeglądarki była odświeżana.
- Wszystkie funkcje w Języku Rust zostały przetestowane na trzech profilach optymalizacyjnych kompilatora.

---

# Pomiary

## JavaScript
- $T_{JS_1}$ --- przed wywołaniem testowanej funkcji
- $T_{JS_2}$ --- po zakończeniu wywołania testowanej funkcji

## Rust
Ponieważ dane przekazywane do funkcji skompilowanej w WebAssembly mogą wymagać serializacji, dokonałem czterech pomiarów dla każdej z nich:
- $T_{RS_1}$ --- przed wywołaniem testowanej funkcji; z poziomu JavaScriptu
- $T_{RS_2}$ --- na początku wywoływanej funkcji; z poziomu Rusta
- $T_{RS_3}$ --- tuż przed zwróceniem wyniku z funkcji; z poziomu Rusta
- $T_{RS_4}$ --- po zakończeniu wywołania testowanej funkcji; z poziomu JavaScriptu

---

# Pomiary (c.d.)

## JavaScript
- $T_{A_{JS}} = T_{JS_2} - T_{JS_1}$

## Rust
- $T_{D_{RS}} = T_{RS_2} - T_{RS_1}$
- $T_{A_{RS}} = T_{RS_3} - T_{RS_2}$
- $T_{S_{RS}} = T_{RS_4} - T_{RS_3}$

---

# Metryki porównawcze
<div></div>

Czas wykonania algorytmu:
- $\overline{T}_{A_{RS}}$ i $\overline{T}_{A_{JS}}$

Całkowity czas wykonania funkcji:
- $T_{C_{RS}} = T_{D_{RS}} + T_{A_{RS}} + T_{S_{RS}}$
- $T_{C_{JS}} = T_{A_{JS}}$

Przyśpieszenie całkowite i algorytmiczne dla języka Rust względem JavaScriptu:
- $S_C = \frac{\overline{T}_{C_{JS}}}{\overline{T}_{C_{RS}}}$
- $S_A = \frac{\overline{T}_{A_{JS}}}{\overline{T}_{A_{RS}}}$



---

# Wyniki badań
Enkodowanie do Base64 (1MB)

<img src="/base64.png" class="h-64 mx-auto object-contain" />

<div class="flex justify-around">
  <div>

  dla $F_1$ i $F_3$:
  - $S_C =  4.8003$
  - $S_A = 11.3087$

  </div>

  <div>

  dla $F_1$ i $F_4$:
  - $S_C =  7.5536$
  - $S_A = 67.5786$
  </div>
</div>

---

# Wyniki badań
Wyliczanie 40-tej liczby Fibonacciego rekurencyjnie

<img src="/fib.png" class="h-64 mx-auto object-contain" />

<div class="flex justify-around">
  <div>

  dla $F_1$ i $F_2$:

  - $S_C = 4.8759$
  - $S_A = 4.8760$

  </div>

</div>

---

# Wyniki badań
Mnożenie macierzy 4x4

<img src="/mat.png" class="h-64 mx-auto object-contain" />

<div class="flex justify-around">
  <div>

  dla $F_1$ i $F_2$:

  - $S_C = 0.1892$
  - $S_A = 1.7500$

  </div>

  <div>

  dla $F_1$ i $F_3$:

  - $S_C = 0.2373$
  - $S_A = 3.5000$

  </div>
</div>

---

# Wyniki badań
Wyliczanie sumy CRC-32 (1MB)

<img src="/crc32.png" class="h-64 mx-auto object-contain" />

<div class="flex justify-around">
  <div>

  dla $F_1$ i $F_2$:

  - $S_C = 3.3466$
  - $S_A = 3.3672$

  </div>

</div>

---

# Wyniki badań
Wyliczanie sumy CRC-64 (1MB)

<img src="/crc64.png" class="h-64 mx-auto object-contain" />

<div class="flex justify-around">
  <div>

  dla $F_1$ i $F_2$:

  - $S_C = 32.6596$
  - $S_A = 32.7962$

  </div>

  <div>

  dla $F_1$ i $F_3$:

  - $S_C = 608.5340$
  - $S_A = 641.7285$

  </div>

</div>

---

# Wyniki badań
API DOM --- Tworzenie elementów (10000)

<img src="/domc.png" class="h-64 mx-auto object-contain" />

<div class="flex justify-around">
  <div>

  dla $F_1$ i $F_2$:

  - $S_C = 0.4024$

  </div>

</div>

---

# Wyniki badań
API DOM --- Aktualizacja co drugiego elementu (10000)

<img src="/domu.png" class="h-64 mx-auto object-contain" />

<div class="flex justify-around">
  <div>

  dla $F_1$ i $F_2$:

  - $S_C = 0.4634$

  </div>

</div>

---

# Wyniki badań
API DOM --- Usuwanie elementów (100)

<img src="/domr.png" class="h-64 mx-auto object-contain" />

<div class="flex justify-around">
  <div>

  dla $F_1$ i $F_2$:

  - $S_C = 0.6076$

  </div>

</div>



---

# Podsumowanie
<div></div>

Przebadanie wydajności języka JavaScript oraz Rust + WebAssembly pozwoliło mi na odpowiedź na pytanie „Czy Rust jest lepszym wyborem dla aplikacji przeglądarkowych niż JavaScript?” jednocześnie osiągając cel pracy:

Dla aplikacji hybrydowych, odwołujących się do WebAssembly jedynie w krytycznych dla wydajności momentach warto pamiętać, by dane były wcześniej przygotowane w taki sposób, by serializacja nie była wymagana, a same dane mogły być bezpośrednio zamieszczone w pamięci modułu WebAssembly.

Jeżeli jednak zastanawiamy się, czy napisać całą logikę po stronie JavaScriptu, czy po stronie WebAssembly, a aplikacja wymaga wydajności i stabilności, WebAssembly może okazać się znacznie lepszym wyborem.
Większe przyśpieszenie algorytmiczne i możliwość wektoryzacji algorytmów dzięki wsparciu dla operacji SIMD obiecują szybsze działanie aplikacji.

Podczas modyfikacji dokumentu należy jednak być ostrożnym i dbać o to, aby nie wysyłać często dużych ilości danych między kontekstami.


---

# Perspektywy dalszych badań
<div></div>

- Dokładniejsze badania nad API DOM aby zrozumieć, skąd dokładnie wynika niska wydajność tych funkcji.
- Przeprowadzenie badań na różnych przeglądarkach aby lepiej zrozumieć różnice w implementacjach WASM.
- Przebadanie wydajności obu języków w środowiskach serwerowych pokroju Node.js i Wasmer.
- Przebadanie wpływu deoptymizacji JIT na wydajność JavaScriptu i porównanie do stałej wydajności WASM.

---
layout: center
---

# Dziekuję za uwagę
