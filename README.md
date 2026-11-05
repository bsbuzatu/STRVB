# STRVB — ghid

Aplicația e un singur fișier: **`index.html`**. Ține datele partajate între toți membrii printr-un proiect **Firebase** (gratis, fără card). Ghidul scurt pentru trupă, cu capturi de ecran, e în **`STRVB-ghid-rapid.pdf`**. Mai jos: ce face fiecare parte a aplicației, apoi pașii de configurare și de publicare.

---

## Ce face fiecare secțiune

### Login
La deschidere, fiecare alege din listă **cine este** și pune **parola**. Userii și parolele sunt în tabelul de la final. După login, dispozitivul ține minte cine ești (până apeși „Ieși").

### Panou trupă (doar de citit)
- **Următoarea repetiție:** data, ora, locul și cine a anunțat că nu poate veni.
- **Cine nu poate veni:** calendarul lunii; o bulină cu inițială într-o zi înseamnă că omul acela **nu poate** atunci. **Rep** / **Conc** / **Înt** marchează repetițiile, concertele și întâlnirile.
- **Ce urmează:** programările viitoare.

### Piese
- Piesele vin din aplicația **Măsura** (cu ordinea concertului activ și măsurile pe părți; titlul, BPM-ul și structura lor se schimbă doar acolo) sau se adaugă manual cu **Adaugă o piesă**: nume, artist (opțional), BPM, link audio și structura (părțile piesei, în ordine).
- **Ascultă** deschide linkul; **Versuri** le arată și le editează.
- **Editează** schimbă datele piesei și, tot de acolo, **arhivează** piesa. Nimic nu se șterge: **Arhivă** ține piesele scoase din repertoriu, iar **Readu în piese** le aduce înapoi.

### Propuneri
- **Propune o piesă:** numele, un link (YouTube sau orice site), opțional BPM-ul și gama. Cel care propune primește automat votul „Îmi place".
- Fiecare membru votează **Îmi place**, **Cum zic și ceilalți** sau **Prefer să nu**. Pe card se vede cine a votat ce, cine a propus piesa, când, și cine a modificat-o ultimul.
- **Editează** schimbă datele propunerii și de acolo o arhivezi; arhiva păstrează și voturile.

### Calendarul tău (aici modifici)
- **Zilele în care nu poți:** apeși pe o zi ca s-o marchezi „nu pot" (roșie); apeși din nou ca s-o eliberezi. Zilele trecute nu se mai pot schimba.
- **Programează:** alegi tipul (Repetiție, Concert sau Întâlnire), data și, opțional, ora și locul. Dacă cineva nu poate în ziua aleasă, apare un avertisment. Programările se pot șterge din lista de sub formular.

Toate datele se sincronizează **live** între toate telefoanele/laptopurile, în câteva secunde.

---

## Configurare Firebase (o singură dată, ~15 min)

### Partea 1 — Creează proiectul
1. Intră pe **https://console.firebase.google.com**, loghează-te cu un cont Google (de preferat unul al trupei).
2. **Add project** -> nume (ex: `trupa-setlist`) -> poți dezactiva Analytics -> **Create**.

### Partea 2 — Adaugă aplicația Web și ia configurarea
3. Pe pagina proiectului, apasă iconița **`</>`** („Web").
4. Nume (ex: `trupa-web`) -> **Register app**. *NU* bifa Hosting.
5. Îți arată un obiect **`firebaseConfig = { ... }`** — copiază-l tot.
6. Deschide **`index.html`** într-un editor de text, caută la începutul scriptului blocul `const firebaseConfig = { apiKey: "PUNE_AICI", ... }` și **înlocuiește valorile** cu cele copiate. Salvează.

   > `apiKey` și restul sunt normale de pus într-un fișier public — nu sunt parole secrete. Datele sunt protejate de regulile de la Partea 5.

### Partea 3 — Baza de date (Firestore)
7. **Build -> Firestore Database -> Create database**.
8. Locația `eur3 (europe-west)` -> **Next**.
9. **Start in production mode** -> **Enable**.

### Partea 4 — Conectarea (Authentication)
10. **Build -> Authentication -> Get started**.
11. La **Sign-in method**, deschide **Anonymous** -> **Enable** -> **Save**.

### Partea 5 — Regulile de securitate (Firestore)
12. **Firestore Database -> tab-ul „Rules"**. Șterge ce e și pune exact:

    rules_version = '2';
    service cloud.firestore {
      match /databases/{database}/documents {
        match /{document=**} {
          allow read, write: if request.auth != null;
        }
      }
    }

   Apasă **Publish**.

**Gata — aplicația funcționează.** Deschide `index.html` în browser, intră cu un user și ar trebui să apară cele 6 piese de pornire.

---

## Publicare — pe GitHub (GitHub Pages)
1. Cont pe **https://github.com** (gratis).
2. **New repository** -> nume (ex: `STRVB`) -> poate fi Private -> **Create**.
3. **Add file -> Upload files** -> trage `index.html` -> **Commit changes**.
4. **Settings -> Pages** -> Source: **Deploy from a branch** -> Branch: **main** / **/(root)** -> **Save**.
5. După ~1 min, apare linkul: `https://NUMELE-TAU.github.io/STRVB/`. Ăsta e linkul pentru trupă.

> Repo Private -> site-ul tot e public la acel link (greu de ghicit). Pentru o trupă e ok.
> Pe telefon, din browser, „Adaugă pe ecranul principal" îl pune ca o iconiță de aplicație.

## Mai târziu — pe hostingul tău
Urcă `index.html` prin File Manager sau FTP în folderul public (de obicei `public_html`). Nimic de schimbat în cod.

## Modificări ulterioare
Editezi `index.html`, îl urci din nou peste cel vechi în repo — site-ul se actualizează singur.

---

## Utilizatori (user / parolă)

| Nume | User | Parolă |
|---|---|---|
| Kara Molnar | Kara | KR |
| Andrei Ardelean | Andrei | AND |
| Nicu Chirteș | Nicu | NC |
| Raul Prodan | Raul | RCP |
| Bogdan Buzatu | Bogo | BO |

Ca să schimbi useri/parole sau culorile membrilor: în `index.html`, caută blocul `const MEMBERS = {` și editează acolo.

---

## Notă de securitate
Userul și parola din login sunt o **poartă de comoditate** — aleg „cine ești" și țin la distanță vizitatorii întâmplători. Nu sunt securitate bancară: parolele sunt scurte și se află în codul paginii, iar cine are linkul se poate conecta la baza de date. Pentru o unealtă privată de trupă e în regulă. Pentru un strat în plus: în **Google Cloud Console -> APIs & Services -> Credentials** poți restricționa cheia API după domeniu (HTTP referrer).

## Probleme frecvente
- **„Nu mă pot conecta la Firebase"** la login -> n-ai activat **Anonymous** (Partea 4) sau n-ai publicat regulile (Partea 5).
- **Piesele nu se salvează / nu apar** -> verifică regulile Firestore (Partea 5) și că Firestore e creat (Partea 3).
- **Nu văd ce face altcineva** -> toți trebuie să folosească **același** link (același proiect Firebase).
