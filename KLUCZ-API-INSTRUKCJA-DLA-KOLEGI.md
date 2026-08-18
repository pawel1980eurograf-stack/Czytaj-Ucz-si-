# Klucz API — instrukcja dla kolegi

Ten plik możesz przesłać razem z aplikacją. Wyjaśnia, po co jest klucz, jak go zdobyć i ile to kosztuje.

---

## Najpierw: czy klucz jest w ogóle potrzebny?

**Nie.** Bez klucza aplikacja działa w całości, offline:

- 5 tekstów do czytania (poziomy A2–B2),
- słownik 3671 haseł — dotknięcie słowa tłumaczy je i wrzuca do powtórek,
- wymowa (głosy systemowe telefonu),
- powtórki metodą Anki, sesje z timerem, plan nauki, statystyki,
- motyw jasny i ciemny, kopie zapasowe.

Klucz włącza dwie dodatkowe rzeczy:

1. **Pisanie nowych tekstów** na dowolny temat, zbudowanych wyłącznie ze słów z listy 3000.
2. **Tłumaczenie słów spoza słownika** — gdyby trafiło się coś, czego w nim nie ma.

Jeśli wystarczą ci teksty wbudowane, po prostu zostaw pole klucza puste i korzystaj.

## Ważne: każdy ma własny klucz

Klucz nie jest zapisany w plikach aplikacji. Trafia wyłącznie do pamięci telefonu, na którym go wklejono.
Osoba, która podzieliła się z tobą aplikacją, nie płaci za twoje użycie i nie ma dostępu do twojego konta —
tak samo jak ty nie masz dostępu do jej.

---

## Jak zdobyć klucz — 5 minut

1. Wejdź na **console.anthropic.com** i załóż konto (e-mail lub konto Google).
2. To jest konto deweloperskie, **osobne od zwykłego Claude.ai** — subskrypcja Claude Pro *nie* daje
   dostępu do API i odwrotnie. Nawet jeśli masz Pro, potrzebujesz tu doładowania.
3. Zakładka **Billing** (Płatności) → **Add credits**. Doładowujesz konto kartą na wybraną kwotę;
   płacisz tylko za to, co faktycznie zużyjesz. Na spokojne używanie tej aplikacji drobna kwota
   wystarczy na bardzo długo — patrz sekcja o kosztach niżej.
4. Zakładka **API keys** → **Create Key** → nazwij go np. `angielski`.
5. **Skopiuj klucz od razu.** Zaczyna się od `sk-ant-…`. Pokazuje się tylko raz —
   po zamknięciu okna nie da się go podejrzeć, trzeba by utworzyć nowy.
6. W aplikacji: zakładka **Postępy → Ustawienia → Klucz API** → wklej → gotowe.

Warto od razu ustawić limit wydatków: **Billing → Usage limits**. Zabezpiecza przed niespodziankami.

## Ile to kosztuje

Rozliczenie jest za tzw. tokeny — w uproszczeniu za ilość przetworzonego tekstu.
Aktualne stawki znajdziesz zawsze na **anthropic.com/pricing**; w sierpniu 2026 model Sonnet 5
kosztował 2 dolary za milion tokenów wejściowych i 10 dolarów za milion wyjściowych.

Co to znaczy w praktyce dla tej aplikacji:

| Czynność | Orientacyjny koszt |
|---|---|
| Napisanie jednego nowego tekstu | ok. 5–10 groszy |
| Przetłumaczenie słowa spoza słownika | ułamek grosza |
| Czytanie, powtórki, wymowa, statystyki | 0 zł — dzieje się offline |

Czyli nawet przy generowaniu tekstu codziennie przez rok mieścisz się w kilkudziesięciu złotych.
Największy koszt to pisanie tekstów, bo aplikacja przesyła wtedy całą listę dozwolonych słów
i sprawdza wynik, czasem prosząc o poprawkę.

## Gdyby coś nie działało

- **„Nie udało się — spróbuj ponownie"** — najczęściej brak środków na koncie albo literówka w kluczu.
  Sprawdź saldo w *Billing* i wklej klucz jeszcze raz (łatwo zgubić znak przy kopiowaniu).
- **Błąd o nieznanym modelu** — w pliku `index.html` znajdź `claude-sonnet-4-6` i wpisz w tym miejscu
  nazwę aktualnego modelu z dokumentacji (**docs.claude.com**). To jedno słowo w jednym miejscu.
- **Brak internetu** — pisanie tekstów wymaga sieci. Reszta aplikacji działa bez niej.

## O bezpieczeństwie

Klucz to jak hasło do twojego konta — kto go ma, może generować na twój rachunek. Dlatego:

- nie wysyłaj go nikomu i nie wklejaj na czacie ani w publicznym repozytorium,
- ustaw limit wydatków,
- gdyby wyciekł, w zakładce *API keys* skasuj go jednym kliknięciem i utwórz nowy.
