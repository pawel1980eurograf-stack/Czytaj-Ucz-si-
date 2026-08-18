# Jak zaktualizować aplikację

Cała aktualizacja robi się z telefonu, przez przeglądarkę. Zajmuje 2 minuty klikania i 5 minut czekania.

---

## Zanim zaczniesz: zrób kopię zapasową

W aplikacji: **Postępy → Kopia zapasowa → Zapisz kopię do pliku**. Plik trafi do Pobranych.
To zabezpieczenie na wypadek, gdyby coś poszło nie tak — w normalnym przebiegu dane zostają nietknięte.

---

## Krok 1 — pobierz nowe pliki

Pobierz na telefon `index.html` (i `package.json`, jeśli akurat się zmienił — powiem ci, kiedy tak jest).

## Krok 2 — podmień plik na GitHubie

Najprościej przez wgranie, bez kopiowania tekstu:

1. Wejdź na **github.com** → twoje repozytorium → folder **www**.
2. **Add file → Upload files**.
3. Wybierz pobrany `index.html`. GitHub sam zauważy, że plik o tej nazwie już istnieje, i go zastąpi.
4. Na dole **Commit changes**.

> Uwaga: upewnij się, że jesteś **wewnątrz folderu `www`**, zanim klikniesz *Upload files*.
> Wgranie do głównego katalogu utworzy drugi plik obok, zamiast podmienić właściwy.

Jeśli zmienił się też `package.json` — jest w głównym katalogu repozytorium, podmieniasz go tak samo.

## Krok 3 — poczekaj na budowanie

Zakładka **Actions** → zobaczysz zadanie *Zbuduj APK* z kręcącym się kółkiem. 4–6 minut.
Zielony ptaszek oznacza gotowe.

## Krok 4 — zainstaluj

Zakładka **Releases** (prawa kolumna strony głównej repozytorium) → najnowsza pozycja →
pobierz **app-debug.apk** → otwórz plik → **Aktualizuj**.

Instalujesz na wierzchu starej wersji. Nie odinstalowujesz jej — wtedy dane zostają na miejscu.

---

## Ważne: klucz podpisu

W repozytorium jest plik `debug.keystore`, a workflow kopiuje go przed budowaniem. Dzięki temu każda
wersja jest podpisana tym samym kluczem i instaluje się jako aktualizacja, z zachowaniem danych.

**Nie kasuj tego pliku i nie zastępuj go innym.** Gdyby zniknął, każde budowanie generowałoby nowy
losowy klucz, a Android odmawiałby instalacji komunikatem „Aplikacja nie została zainstalowana" —
jedynym wyjściem byłoby odinstalowanie starej wersji razem z postępami.

Jeśli już wcześniej zainstalowałeś APK zbudowane **bez** tego pliku, jednorazowo czeka cię:

1. kopia zapasowa w aplikacji (*Postępy → Zapisz kopię do pliku*),
2. odinstalowanie starej aplikacji,
3. instalacja nowego APK,
4. *Postępy → Wczytaj kopię z pliku*.

Od tej pory kolejne aktualizacje pójdą już gładko, bez odinstalowywania.

---

## Aktualizacja wersji przez przeglądarkę (GitHub Pages)

Jeśli używasz wersji dodanej do ekranu głównego zamiast APK, wystarczy krok 2 — podmieniasz `index.html`
i po minucie masz nową wersję. Jedno ale: przeglądarka trzyma starą kopię w pamięci podręcznej.
Dlatego przy każdej zmianie podnieś numer w pliku `www/sw.js`:

```js
const V = "czytaj-ucz-sie-v2";   // było v1
```

Bez tego telefon może jeszcze przez jakiś czas pokazywać poprzednią wersję.

---

## Skąd wiadomo, że nowa wersja weszła

W zakładce **Postępy**, na samym dole, jest linijka z liczbą haseł w słowniku. Jeśli w aktualizacji
zmieniało się coś widocznego — nowy przycisk, inny tekst — po prostu to zobaczysz. W razie wątpliwości
zamknij aplikację całkowicie (usuń z listy ostatnich) i otwórz ponownie.
