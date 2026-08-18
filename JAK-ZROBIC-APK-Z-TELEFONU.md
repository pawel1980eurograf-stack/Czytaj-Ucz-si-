# Jak zrobić APK z samego telefonu

Bez komputera, bez instalowania niczego. GitHub zbuduje aplikację za ciebie w chmurze i odda gotowy plik.
Cała zabawa to jakieś 10 minut, z czego 5 to czekanie.

---

## Krok 1 — rozpakuj ZIP na telefonie

Otwórz aplikację **Pliki** → Pobrane → dotknij `czytaj-i-ucz-sie-android.zip` → **Wypakuj**.
Powstanie folder `projekt` z plikami w środku.

## Krok 2 — załóż konto na GitHubie

Wejdź na **github.com** w Chrome → *Sign up*. Wystarczy e-mail i hasło. Konto jest darmowe.

## Krok 3 — nowe repozytorium

1. Po zalogowaniu dotknij **+** (prawy górny róg) → **New repository**.
2. *Repository name*: `angielski` (albo cokolwiek).
3. Zostaw **Public**.
4. Zaznacz **Add a README file**.
5. **Create repository**.

## Krok 4 — wgraj pliki

W repozytorium: **Add file → Upload files**.

Wgrywaj w takiej kolejności (każdą partię osobno (po każdej dotknij *Commit changes*):

**Partia 1** — z folderu `projekt` zaznacz i wgraj:
`package.json`, `capacitor.config.json`, `icon-source.png`

**Partia 2** — cały folder `www`. Jeśli telefon nie pozwala wybrać folderu, wgraj pojedynczo pliki:
`index.html`, `manifest.json`, `sw.js`, `icon-192.png`, `icon-512.png`
Wtedy **przed** wgraniem wpisz w polu nazwy `www/` — GitHub sam utworzy folder.

**Partia 3** — plik budujący. To najważniejszy krok:
dotknij **Add file → Create new file**, w polu nazwy wklej dokładnie:

```
.github/workflows/build-apk.yml
```

a w duże pole poniżej wklej całą zawartość pliku `build-apk.yml` z folderu `projekt/.github/workflows/`.
(Otwórz go w Plikach jako tekst, zaznacz wszystko, skopiuj.) Na koniec **Commit changes**.

## Krok 5 — poczekaj

Po wgraniu ostatniego pliku budowanie rusza samo. Wejdź w zakładkę **Actions** — zobaczysz zadanie
*Zbuduj APK* z kręcącym się kółkiem. Trwa 4–6 minut. Zielony ptaszek = gotowe.

Jeśli zamiast ptaszka pojawi się czerwony krzyżyk, dotknij zadania, zrób zrzut ekranu z błędem
i pokaż mi — poprawię.

## Krok 6 — pobierz i zainstaluj

Wejdź w zakładkę **Releases** (prawa kolumna strony głównej repozytorium) → najnowsza pozycja →
pobierz **app-debug.apk**.

Otwórz pobrany plik. Android zapyta o zgodę na *instalowanie z nieznanych źródeł* — zezwól
(to standardowe pytanie przy aplikacjach spoza Sklepu Play; instalujesz własny plik).

Gotowe. Ikona pojawi się na liście aplikacji.

---

## Jak później wprowadzać zmiany

Nie musisz nic wgrywać od nowa:

1. W repozytorium wejdź w `www/index.html` → ikona ołówka → popraw → *Commit changes*.
2. GitHub sam zbuduje nowe APK.
3. Za kilka minut pobierz je z **Releases** i zainstaluj na wierzchu starego — dane zostają.

## Bonus: wersja przez przeglądarkę, od ręki

Nie chcesz czekać na APK? W repozytorium: **Settings → Pages** → *Source: Deploy from a branch*,
branch `main`, folder `/root` → **Save**. Po minucie dostaniesz adres w stylu
`twojanick.github.io/angielski/www/`. Otwórz go w Chrome → menu ⋮ → *Zainstaluj aplikację*.

Dane trzymają się tam na stałe (w przeciwieństwie do otwierania pliku z Pobranych),
działa offline. Brakuje tylko powiadomień przy zablokowanym telefonie — te są wyłącznie w APK.
