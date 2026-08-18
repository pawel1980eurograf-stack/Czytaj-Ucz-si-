# Jak zainstalować na iPhone'a

Na iPhonie są dwie drogi. Zacznij od pierwszej — jest darmowa, zajmuje 3 minuty i nie wymaga komputera.

---

# Droga A: instalacja przez Safari (bez komputera, 3 minuty)

Apple pozwala instalować aplikacje internetowe prosto z Safari. Dostajesz ikonę na ekranie,
tryb pełnoekranowy bez paska przeglądarki i działanie bez internetu.

## Co jest potrzebne

Aplikacja musi być pod jakimś adresem — otwieranie pliku z Plików na iPhonie nie zadziała.
Najprościej postawić ją na GitHub Pages, wszystko przez przeglądarkę telefonu:

1. Wejdź na **github.com** → *Sign up* (darmowe konto).
2. **+** w prawym górnym rogu → **New repository** → nazwa np. `angielski` → zostaw **Public** →
   zaznacz *Add a README file* → **Create repository**.
3. **Add file → Upload files** → wgraj pliki z folderu `www`:
   `index.html`, `manifest.json`, `sw.js`, `icon-192.png`, `icon-512.png` → **Commit changes**.
4. **Settings → Pages** → *Source: Deploy from a branch*, branch `main`, folder `/root` → **Save**.
5. Odczekaj minutę i odśwież tę stronę — pojawi się adres w stylu
   `https://twojanick.github.io/angielski/`.

## Instalacja na telefonie

1. Otwórz ten adres **w Safari** (nie w Chrome — na iPhonie tylko Safari potrafi instalować).
2. Dotknij przycisku **udostępniania** (kwadrat ze strzałką w górę, na dole ekranu).
3. Przewiń listę i wybierz **Dodaj do ekranu początkowego**.
4. Nazwa *Czytaj&Ucz* → **Dodaj**.

Gotowe. Ikona jest na ekranie, aplikacja otwiera się bez paska Safari i działa offline.
Ten sam adres wyślij koledze — u niego instalacja wygląda tak samo.

## Co działa, a co nie

| | Droga A (Safari) |
|---|---|
| Teksty, słownik 3671 haseł, powtórki, statystyki | tak, offline |
| Wymowa | tak, głosy systemowe iPhone'a |
| Motyw jasny/ciemny, plan nauki, sesje z timerem | tak |
| Przypomnienia przy zablokowanym telefonie | **nie** — tylko przy otwartej aplikacji |
| Pisanie nowych tekstów | tak, po wklejeniu własnego klucza API |

**Ważne — zrób kopię zapasową.** iOS potrafi usunąć dane aplikacji internetowej, jeśli nie używasz jej
przez dłuższy czas. W zakładce *Postępy → Kopia zapasowa* jest przycisk **Zapisz kopię do pliku** —
rób to raz na jakiś czas, a plik wrzuć do iCloud Drive. Odtworzenie to jedno dotknięcie *Wczytaj kopię*.

---

# Droga B: prawdziwa aplikacja (wymaga Maca)

Tu nie ma obejścia: **aplikację na iPhone'a da się zbudować wyłącznie na komputerze Mac z Xcode.**
Apple nie udostępnia narzędzi na Windows ani Linuxa, a serwisy budujące w chmurze i tak wymagają
płatnego konta dewelopera do podpisania aplikacji. Jeśli nie masz dostępu do Maca — zostań przy drodze A,
różnica to w praktyce tylko przypomnienia przy zablokowanym ekranie.

## Kroki na Macu

```bash
# jednorazowo: Xcode ze sklepu App Store + narzędzia
sudo xcode-select --install
sudo gem install cocoapods

cd czytaj-i-ucz-sie
npm install
npx cap add ios
npx cap sync ios
npx cap open ios
```

Ostatnia komenda otworzy Xcode.

1. W panelu po lewej kliknij **App** → zakładka **Signing & Capabilities**.
2. **Team**: zaloguj się swoim Apple ID (*Add an Account…*). Zwykłe, darmowe konto wystarczy.
3. **Bundle Identifier** zmień na własny, np. `pl.twojenazwisko.angielski` — musi być unikalny.
4. W **Capabilities** dodaj **Push Notifications** oraz **Background Modes → Remote notifications**
   (potrzebne dla przypomnień).
5. Podłącz iPhone'a kablem, wybierz go na górnej belce jako cel i naciśnij **▶ Run**.
6. Na telefonie: *Ustawienia → Ogólne → VPN i zarządzanie urządzeniem* → zaufaj swojemu profilowi.

### Haczyk z darmowym kontem

Aplikacja podpisana darmowym Apple ID **wygasa po 7 dniach** — potem trzeba ją wgrać ponownie
przez Xcode. Żeby działała rok, potrzebny jest płatny Apple Developer Program (99 USD rocznie).
Dla jednej osobistej aplikacji to zwykle nie ma sensu; darmowe konto wystarczy, jeśli masz Maca pod ręką
i nie przeszkadza ci odświeżanie co tydzień.

### Udostępnienie koledze

Przez Xcode zainstalujesz aplikację tylko na telefonie podłączonym do tego Maca. Żeby wysłać ją komuś
zdalnie, potrzebny jest płatny program deweloperski i TestFlight. Dlatego dla kolegi
**najrozsądniejszy jest zwykły link z drogi A** — wysyłasz adres, on dodaje do ekranu początkowego i gotowe.

---

## Co z kluczem API przy udostępnianiu

Klucz nie jest zapisany w plikach aplikacji — siedzi wyłącznie w pamięci telefonu, na którym go wklejono.
Kolega dostanie puste pole i zdecyduje sam: wkleja swój klucz albo zostawia puste i używa wersji offline.
Twoje konto nie jest w to zamieszane w żaden sposób.

Gdybyś kiedyś chciał, żeby wszyscy korzystali z jednego konta, jedyny bezpieczny sposób to postawić
własny serwer pośredniczący i podmienić adres w funkcji `claude()` w pliku `index.html`.
Nigdy nie wpisuj klucza na stałe w kod — każdy, kto ma aplikację, mógłby go z niej wyjąć.
