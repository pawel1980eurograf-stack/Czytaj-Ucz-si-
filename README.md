# Czytaj i ucz się — apka na Androida

Nauka angielskiego przez czytanie: dotykasz nieznanego słowa, zamienia się na polskie i wpada do powtórek metodą Anki.

Cała aplikacja to **jeden plik**: `www/index.html`. Tam siedzi wygląd, teksty, słownik 3671 haseł i cała logika. Capacitor tylko opakowuje go w APK i daje dostęp do powiadomień systemowych oraz trwałego zapisu.

---

## 1. Co musisz mieć na komputerze

| Narzędzie | Po co | Skąd |
|---|---|---|
| Node.js 20+ | budowanie projektu | nodejs.org |
| Android Studio | SDK, emulator, podpisywanie APK | developer.android.com/studio |
| JDK 21 | idzie w komplecie z Android Studio | — |

W Android Studio: **SDK Manager → zainstaluj Android SDK Platform 34 i Build-Tools**.

## 2. Pierwsze uruchomienie (raz)

```bash
cd czytaj-i-ucz-sie
npm install
npx cap add android
npx cap sync
```

Powstanie katalog `android/` — to natywny projekt. Nie edytuj w nim nic ręcznie poza tym, co opisane niżej; przy `cap sync` część rzeczy się nadpisuje.

## 3. Zbudowanie APK

**Najprościej — z Android Studia:**
```bash
npx cap open android
```
W Studiu: *Build → Build Bundle(s)/APK(s) → Build APK(s)*. Plik ląduje w
`android/app/build/outputs/apk/debug/app-debug.apk` — przerzuć go na telefon i zainstaluj (trzeba zezwolić na instalację z nieznanych źródeł).

**Albo z terminala:**
```bash
npm run apk
```

**Wersja do Sklepu Play** wymaga podpisanego wydania:
```bash
keytool -genkey -v -keystore moj-klucz.keystore -alias czytaj -keyalg RSA -keysize 2048 -validity 10000
```
Potem w `android/app/build.gradle` dodaj `signingConfigs` z tym kluczem i uruchom `npm run apk:release`. Keystore trzymaj poza repozytorium — bez niego nie wypuścisz aktualizacji.

## 4. Jak wprowadzać poprawki

Cały cykl zmian wygląda tak:

```bash
# 1. edytujesz www/index.html
# 2. wgrywasz zmiany do projektu natywnego
npx cap sync
# 3. budujesz nowe APK
npm run apk
```

Podczas dłubania szybciej jest testować w przeglądarce — bez budowania:
```bash
npx serve www      # albo: python3 -m http.server 8080 --directory www
```
Otwórz `http://localhost:8080` i sprawdzaj na bieżąco. Działa wszystko poza powiadomieniami systemowymi (w przeglądarce lecą tylko przy otwartej karcie).

### Gdzie co siedzi w `index.html`

Plik jest podzielony komentarzami. Szukaj tych miejsc:

| Chcesz zmienić | Szukaj | Uwagi |
|---|---|---|
| Kolory, czcionki, wygląd | `:root{` i `body.dark{` | dwa motywy, te same nazwy zmiennych |
| Teksty do czytania | `const BUILTIN=[` | dopisz `{id:"t6",lvl:"B1",title:"…",body:\`…\`}` |
| Słownik | `const RAW = \`` | format `słowo=tłumaczenie|`, kolejność dowolna |
| Formy gramatyczne (walked → walk) | `function forms(` | tu dodasz nowe końcówki |
| Algorytm powtórek | `function nextIvl(` | odstępy SM-2 |
| Cel dzienny | `dailyGoal()` | domyślnie 6 fiszek na minutę |
| Godziny przypomnień | `function slots(` | kolejne sesje co 4 h |
| Zapis danych | `const Store={` | jedno miejsce, cała reszta z niego korzysta |

Po zmianie `index.html` podnieś numer wersji w `www/sw.js` (`const V = "czytaj-ucz-sie-v2"`), inaczej wersja PWA może serwować starą kopię z cache. W APK to nie ma znaczenia.

### Dopisywanie słów do słownika

Kolejność bez znaczenia, duplikaty są odsiewane przy starcie. Wystarczy dokleić na końcu bloku:

```
mindfulness=uważność|burnout=wypalenie|
```

Uwaga: generator nowych tekstów pilnuje, żeby używać **wyłącznie** słów z tej listy — więc każde dopisane hasło od razu poszerza to, o czym aplikacja może pisać.

## 5. Powiadomienia

W APK są to prawdziwe powtarzalne powiadomienia Androida (`@capacitor/local-notifications`) — docierają przy zablokowanym telefonie. Ustawiane są automatycznie po każdej zmianie planu w zakładce *Postępy*.

Dwie rzeczy warte sprawdzenia na telefonie, jeśli nie przychodzą:
- **Android 13+** wymaga zgody na powiadomienia — apka o nią prosi przy włączaniu planu.
- **Oszczędzanie baterii** (szczególnie Xiaomi, Samsung, Huawei) potrafi ubić harmonogram. Ustawienia → Aplikacje → Czytaj i ucz się → Bateria → *bez ograniczeń*.

Własna ikonka powiadomienia: wrzuć białą sylwetkę na przezroczystym tle jako
`android/app/src/main/res/drawable/ic_stat_icon.png` (24×24 dp, wersje w mdpi–xxhdpi).

## 6. Ikona aplikacji

`icon-source.png` (1024×1024) to źródło. W Android Studio: prawy klik na `app` → *New → Image Asset* → wskaż ten plik → wygeneruje wszystkie rozmiary.

## 7. Klucz API — do czego i czy trzeba

Aplikacja działa **w pełni offline**: 5 tekstów, słownik 3671 haseł, wymowa (głosy systemowe Androida), powtórki, statystyki. Klucz Anthropic jest opcjonalny i włącza dwie rzeczy: pisanie nowych tekstów na dowolny temat oraz tłumaczenie słów spoza listy.

Wklejasz go w *Postępy → Ustawienia*, zapisuje się tylko na telefonie. Miej świadomość, że klucz w aplikacji na telefonie da się z niej wydobyć — jeśli apka miałaby trafić do innych ludzi, zamiast tego postaw mały serwer pośredniczący i podmień adres w funkcji `claude()`.

## 8. Struktura projektu

```
czytaj-i-ucz-sie/
├─ www/
│  ├─ index.html      ← cała aplikacja (edytujesz tylko to)
│  ├─ manifest.json   ← nazwa, ikony, kolory (też dla PWA)
│  ├─ sw.js           ← cache offline dla wersji przeglądarkowej
│  └─ icon-192/512.png
├─ capacitor.config.json  ← nazwa apki, appId, wtyczki
├─ package.json
├─ icon-source.png
└─ android/           ← powstaje po `npx cap add android`
```

## 9. Bez Android Studia — wersja PWA

Jeśli nie chcesz się bawić w budowanie: wrzuć zawartość `www/` na dowolny hosting z HTTPS (GitHub Pages, Netlify), otwórz w Chrome na telefonie i *Dodaj do ekranu głównego*. Dostajesz ikonę, tryb pełnoekranowy i działanie offline. Odpadają tylko powiadomienia przy zablokowanym telefonie.
