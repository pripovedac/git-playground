# Komande 

Zaista nemam zelju da previse detaljno nesto opisujem 
(i iskreno se nadam da cu ostati pri tome), cisto je ideja
da u kratkim crtama zapisem po koju stvar koju sam naucio citajuci
knjigu, ili, eto, nesto sto smatram zanimljivim.

## Osnove

### Komitovanje

Imaj na umu da skoro sve sto je komitovano moze da se povrati. Cak i ako je grana obrisana.

- `$git commit -a` - dodaje samo _track_-ovane fajlove preskacuci stejdzing.
Ovo dodatno znaci da novokreirani fajlovi nece biti dodati jer nisu _track_-ovani.
Fajl se smatra _track_-ovanim u slucaju da je po nadzorom sistema kontrole verzije,
te je i logicno sto novi fajlovi ne spadaju ovde.

- `git commit --amend` - moze da sluzi da dodas zaboravljene fajlove u poslednji
komit, ili ako nista nisi menjao od fajlova, samo poruku da promenis. Vise na sajtu.

### Uklanjanje fajlova

- `$git rm <file>` - komanda koja uklanja fajl sa gita.
Nakon ove komande i komitovanja uklonjeni fajl se vise nece nalazi ni lokalno, niti
ce ga git pratiti. Korisni flegovi su: `-f` i `--cached`. U slucaju prvog fajl ce biti
uklonjen iako je trenutno u stejdzingu. Trajno uklonjen. Zato i mora `-f`, da te spreci 
da ne zajebes stvari.
Ako izaberes drugu opciju, `--cached` onda ce samo iz stejdzinga u _working tree_ da se prebaci, odnosno nece ti ulaziti u 
naredni komit. Ovo je i te kako korisno. Taj fajl vise nije trekovan.
Inace, kao ime fajla nije neophodno unositi samo ime konkretno ime fajla, ili foldera,
vec je moguce da to budu i neki sabloni.
Moguce je fajl skinuti sa stejdza i sa `$git reset HEAD <file>`, ali tada ce fajl i dalje
biti trekovan, za razliku od komande `rm` kada to vise nije.

### Alijasi

Prilikom cestog koriscenja (dugih) komandi povoljno je kreiranje alijasa:

- `$git config --global alias.<alias_name> <command>`

e.g.

- `$ git config --global alias.unstage 'reset HEAD --' ` - ovde su navodnici kod komande potrebni jer
imamo razmake u okviru nje. Upotreba bi bila `$git unstage fileA`.

### Istorija

- `$git log` je osnovna komanda kojoj se dodaju odredjeni modifikatori u cilju preciziranja izvestaja.
Prolazeci kroz knjigu dodavacu one koje smatram korisnim.
    - `--oneline --decorate` koncizan prikaz gde grane trenutno pokazuju
Imaj na umu da samo navodjenje komande _bez definisanje grane_ daje istoriju od pointera te grane u proslost. Odnosno ako neka grana
prednjaci u odnosu na pointer gde smo sada, to se nece videti.
    - `<ime_grane>` uvid u istoriju konkretne grane. Ukoliko zelimo da vidimo sve grane, fleg je potreban ti je fleg `--all`
    - `--graph` prikaz u vidu grafa

## Grananje

Kreiranje grane je prosto kreiranje pointera na odgovarajuci komit. Komit je takodje pointer na stablo
koje ukazuje na blob-ove odnosno promenjene fajlove, `snapshot`. Koja je grana trenutno aktivna, odredjujemo
pomocu `HEAD` pointera.
Grane su izuzetno jefitne. To je mali fajl od 40 karaktera SHA-1 ceksume koji ukazuju na odgovarajuci komit.
Jedna grana je 41B, 40 karaktera + 1 bajt za novu liniju.

Pregled grana koje su spojene u trenutnu granu je moguce uraditi sa:

- `$git branch --merged` 

ili one koje nisu spojene:

- `$git branch --no-merged`

Ovo je korisno recimo kad smo na `master` grani pa da proverimo koje grane su spojene kako bismo mogli da 
ih obrisemo. Ili da vidimo na cemu se jos uvek radi.

### Spajanje (_eng._ merge)

Spajanje se vrsi tako sto odes na granu u koju hoces da spojis neku drugu granu komandom:

- `#git merge <branch_name>`

Tada se desava jedan od dva scenarija.

Ako je grana u koju spajamo nije divergirala od spojene grane onda se samo pomeri pokazivac
grane u koju spajamo, tzv. _fast forwarding_. U suprotnog se generise komit spajanja.

### Rebaziranje

Flou kad se radi rebaziranje sa ciljem spajanja je da nakon izvrsenog uspesnog rebaziranja
uradimo spajanje istom onom komandom za merge-ovanje. Tada pomeramo pointer grane u 
koju smo spojili unapred. Tj. mi smo rebaziranjem omogucili _fast-forwarding_.
A onda sve to pushujemo kako bismo i remote pointer grane u koju smo spojili pomerili unapred.

Kako se uspesno radi rebaziranje u nastavku:

Ukapirali smo vec da kad se radi `$git rebase <base_branch>` da zapravo menjamo _bazu_ grane na kojoj
smo trenutno sa granom koju navodimo kao argument komande. Onda smo isto tako ukapirali da je
proces rebaziranja takav da se svi nasi novi komitovi, jedan po jedan, lepe na navedenu osnovu.
Sa svakim komitom postoji mogucnost nastajanja konflikata koji se nakon resavanja potvrdjuju sa:

```
$git add <file_name> 
$git rebase --continue 
```

Ako izaberes sve promene sa grane na koju rebaziras, tu ume da se nadje jedna poruka koja je malo zbunjujuca, 
a koja kaze da promene nisu detektovane i bivas pitan da li si uradio prethodne dve komande 
koje jesi bukvalno upravo uradio. :D
To je zato sto `rebase` ocekuje neku promenu usled komita koji je sada naisao, a ti si ga odbacio.
Zato u takvoj situaciji, kada ne zelis da prihvatis promene sa grane na kojoj si (nove promene), treba
da uradis komandu:

- `$git rebase --skip`

Kada zavrsimo sa resavanjem konflikata neophodno je da pusujemo sa flegom `--force`. Zasto?
Zato sto `push` u pozadini funkcionise kao `fast-forward`, a istorija nase lokalne
grane i grane na serveru vise nije linearna.
Svi komitovi su prebaceni na granu nad kojom smo rebazirali, a ti neki komitovi na serveru su
ostali da _vise_.

- - base branch - - local foo branch commits
        \
        server branch foo

Zato ne mozemo prostim `fast-forward` da spojimo `local foo` i `server foo`.

Kada zavrsimo rebaziranje lokalni komitovi ce biti ispred <base_branch> tako da moramo da 
se prebacimo na nju i uradimo
`$git merge <rebased_branch>` kako bismo premotali unapred pokazivac osnovne grane.