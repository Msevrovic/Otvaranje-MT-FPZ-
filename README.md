# Zahtjev za otvaranje konto kartice projekta — FPZ

Web forma za prikupljanje zahtjeva za otvaranje novog mjesta troška (MT) prema novim šiframa donesenim odlukom Voditelja financijsko-računovodstvene službe FPZ-a (28.04.2026.).

## Sadržaj

- `index.html` — samostalna HTML forma s ugrađenim FPZ banerom (base64), spremna za GitHub Pages
- `assets/fpz_banner.png` — standalone banner kao rezerva (forma ga inače ne treba jer je inline)

## Kako radi mailing

Forma šalje unose preko **FormSubmit.co** (besplatan servis, bez registracije) na `sevrovic@gmail.com`. Konfiguracija je u skrivenim poljima na vrhu `<form>` taga u `index.html`:

```html
<form action="https://formsubmit.co/sevrovic@gmail.com" method="POST">
  <input type="hidden" name="_subject" value="Novi zahtjev: Konto kartica projekta">
  <input type="hidden" name="_template" value="table">
  <input type="hidden" name="_captcha" value="true">
```

### Aktivacija (jednom)

1. Deployaj formu na neki javni URL (vidi dolje)
2. Ispuni i pošalji jedan testni zahtjev
3. FormSubmit ti pošalje mail s linkom za potvrdu — klikni "Confirm"
4. Od tada svi unosi dolaze ravno u inbox kao tablica

## Deploy na GitHub Pages

```bash
# u folderu repozitorija
git add .
git commit -m "Forma za zahtjev otvaranja konto kartice"
git push
```

Zatim u GitHub repo: **Settings → Pages → Source: main → / (root) → Save**.

Nakon par minuta forma je dostupna na `https://<username>.github.io/<repo-name>/`.

## Polja u formi

- **Podnositelj:** ime i prezime, email, zavod/katedra, telefon
- **Projekt:** naziv, voditelj, akronim, datum početka i završetka
- **Klasifikacija:** šifra MT (padajući izbornik sa svih 23+ kodova iz odluke), izvor financiranja, aktivnost
- **Financije:** ukupna vrijednost (EUR), iznos za FPZ, sufinanciranje
- **Dodatno:** poveznica na ugovor/odluku, napomena

## Promjena email primatelja

U `index.html` zamijeni:

```
action="https://formsubmit.co/sevrovic@gmail.com"
```

s tvojom novom adresom (npr. `sevrovic@fpz.unizg.hr`).

## Verzije

- **v0.1** — osnovna forma s navy headerom
- **v0.2** — dodan FPZ F-znak + tekst u zaglavlju
- **v0.3** — pravi FPZ banner (rekreiran iz F-znaka iz potpisa Filipovog maila + 4 reda navy teksta)
- **v0.4** — cascading dropdowns (kad odabereš MT, predloženi izvor i aktivnost se filtriraju + auto-popunjavaju iz `MT_RULES` mape), katalog izvora financiranja iz `Izvori-financiranja---KONTA` GitHub Pages stranice, automatski izračun "Iznos za FPZ" iz ukupne vrijednosti × postotak (default 100%, 21% za 20+1% projekte), gumb za reset auto-iznosa nakon ručnog unosa. Sva polja ostaju editable — sugestije, ne hard limits.
