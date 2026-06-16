# INSTRUKCJA WDROŻENIA — landing aicombo.pl

Całość to **jeden statyczny plik `index.html`** (zero zależności, zero builda). Wystarczy go umieścić na dowolnym hostingu i podpiąć domenę.

---

## 1. Wstaw GA4 Measurement ID (2 minuty)

1. Wejdź do Google Analytics → Administracja → Strumienie danych → wybierz strumień web (lub utwórz dla `aicombo.pl`).
2. Skopiuj **Measurement ID** (format `G-ABC123XYZ`).
3. W `index.html` znajdź `G-XXXXXXXXXX` — występuje w **DWÓCH miejscach** w sekcji `<head>` (w `src` skryptu gtag.js oraz w `gtag('config', ...)`) — i podmień w obu.

## 2. Podmień adres e-mail (1 minuta)

Znajdź `kontakt@aicombo.pl` — występuje w **DWÓCH miejscach** (stopka + modal „Zarezerwuj demo") — i podmień na realny adres. W modalu zachowaj format `mailto:` z parametrem `?subject=` (możesz zmienić temat).

## 3. Podmień placeholdery wideo (gdy klipy będą gotowe)

1. Utwórz obok `index.html` folder `media/` i wgraj pliki: `demo-01.mp4` … `demo-06.mp4` (pionowe 9:16, najlepiej H.264, do ~10 MB/szt.; opcjonalnie miniatury `demo-01.jpg` jako `poster`).
2. W sekcji `<!-- 3. GALERIA DEMO -->` dla każdej karty:
   - **usuń** linijkę `<div class="demo-ph">…</div>`,
   - **odkomentuj** gotowy szablon `<video>…</video>` pod spodem i sprawdź ścieżkę pliku.
3. Style dla `<video>` są już w CSS — proporcje 9:16 i zaokrąglenia zadziałają same. Możesz podmienić dowolną liczbę kart (np. tylko 3) — resztę zostaw jako placeholdery.

Hero (ramka z shimmerem) celowo pozostaje dekoracyjna — nie trzeba jej podmieniać.

## 4. Wdrożenie pod domenę aicombo.pl (wybierz jedną z opcji)

**Opcja A — Cloudflare Pages (zalecana, darmowa):**
1. dash.cloudflare.com → Workers & Pages → Create → Pages → Upload assets → wgraj `index.html` (i folder `media/`).
2. Po deployu: Custom domains → dodaj `aicombo.pl` → Cloudflare poprowadzi przez rekordy DNS (jeśli domena jest już w Cloudflare — jeden klik).

**Opcja B — Netlify (darmowa):**
1. app.netlify.com → Add new site → Deploy manually → przeciągnij folder z `index.html`.
2. Domain settings → Add custom domain → `aicombo.pl` → ustaw u rejestratora rekordy wg wskazówek (A → 75.2.60.5 lub CNAME na subdomenę netlify).

**Opcja C — dotychczasowy hosting (FTP):**
Wgraj `index.html` (+ `media/`) do katalogu głównego domeny (zwykle `public_html/`). Nic więcej.

W każdej opcji upewnij się, że działa **HTTPS** (Cloudflare/Netlify włączają automatycznie).

## 5. Test śledzenia (przed startem publikacji!)

1. Otwórz stronę z testowym linkiem UTM, np.:
   `https://aicombo.pl/?utm_source=instagram&utm_medium=organic&utm_campaign=poc_demo&utm_content=p01_a`
2. W GA4: **Administracja → DebugView** (na czas testu dodaj do adresu `&debug_mode=1` albo użyj rozszerzenia GA Debugger).
3. Kliknij oba przyciski CTA i sprawdź, czy przychodzi event **`cta_click`** z parametrami `cta_variant`, `cta_location`, `utm_content` (powinno pokazać `p01_a`).
4. Aby `utm_content` i `cta_variant` były widoczne w raportach (nie tylko w DebugView): GA4 → Administracja → Definicje niestandardowe → **Utwórz wymiar niestandardowy** dla parametrów zdarzenia: `utm_content`, `cta_variant`, `cta_location`, `utm_source` (zakres: zdarzenie). Zrób to od razu — wymiary działają od momentu utworzenia, wstecz nie.

## 6. Baner zgody — jak to działa (nic nie musisz robić)

Strona używa **Consent Mode v2**: domyślnie cookies analityczne są wyłączone. Gdy użytkownik kliknie „Zgadzam się" — pomiar pełny; gdy „Tylko bez cookies" — zdarzenia (w tym `cta_click`) nadal są wysyłane jako anonimowe pingi bez cookies, więc kliknięcia do badania liczą się w obu przypadkach. Do pracy dyplomowej możesz dopisać zdanie, że pomiar zaprojektowano zgodnie z zasadą minimalizacji danych (brak danych osobowych, brak formularzy, dobrowolny mailto).

## 7. Checklist przed pierwszym postem z linkiem w bio

- [ ] GA4 ID podmienione (2 miejsca), event `cta_click` widoczny w DebugView
- [ ] Wymiary niestandardowe utworzone (pkt 5.4)
- [ ] E-mail podmieniony (2 miejsca)
- [ ] HTTPS działa, strona ładuje się na telefonie
- [ ] Link w bio IG/TikTok ma poprawny UTM wg schematu z pliku `04_Posty_demo_PL.md`
- [ ] Zrzut ekranu strony (hero + stopka) → załącznik pracy jako dowód ujawnienia AI
