# Komande 

Zaista nemam zelju da previse detaljno nesto opisujem 
(i iskreno se nadam da cu ostati pri tome), cisto je ideja
da u kratkim crtama zapisem po koju stvar koju sam naucio citajuci
knjigu, ili, eto, nesto sto smatram zanimljivim.

## Komitovanje

- `$git commit -a` - dodaje samo _track_-ovane fajlove preskacuci stejdzing.
Ovo dodatno znaci da novokreirani fajlovi nece biti dodati jer nisu _track_-ovani.
Fajl se smatra _track_-ovanim u slucaju da je po nadzorom sistema kontrole verzije,
te je i logicno sto novi fajlovi ne spadaju ovde.

- `git commit --amend` - moze da sluzi da dodas zaboravljene fajlove u poslednji
komit, ili ako nista nisi menjao od fajlova, samo poruku da promenis. Vise na sajtu.

## Uklanjanje fajlova

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

## Rebaziranje

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

## Alijasi

Prilikom cestog koriscenja (dugih) komandi povoljno je kreiranje alijasa:

- `$git config --global alias.<alias_name> <command>`

e.g.

- `$ git config --global alias.unstage 'reset HEAD --' ` - ovde su navodnici kod komande potrebni jer
imamo razmake u okviru nje. Upotreba bi bila `$git unstage fileA`.