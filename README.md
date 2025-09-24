## Scrapper Chronologického registra Slov-Lex ##
Scraper parsuje zoznamy legislatívnych predpisov SR dostupných na https://www.slov-lex.sk/pravne-predpisy
Vytvorí z nich HASH a následne umožní vypísanie vo forme CSV.

Webové rozhranie Slov-Lex má čistú formu a umožňuje ľahko parsovať webový obsah.

Dáta referencuje cez XPath `//table[@id="YearTable"]/tbody/tr`
Na parsovanie používa Perl modul https://metacpan.org/pod/Web::Scraper

Je rozšírovaný na spracovanie štruktúr jednotlivých znení legislatívnych predpisov.
Aktuálna verzia sťahuje informácie o historických zneniach/vydaniach legislatívnych predpisov.
Výstup môže byť použitý ako referenčný zoznam pre referencie v IS.

### Interné dátové štruktúry ###
```
%x->{$rok}->{lexs}=>[
                  {
                  index=>"číslo_predpisu/yyyy",
                  type=>"typ_predpisu",
                  fullname=>"plný_názov_predpisu",
                  uri=>URI objekt "Slov-Lex URI predpisu",
                  "info" : {
                     "autor" : "Ministerstvo financií Slovenskej republiky",
                     "dátum schválenia" : "dd.mm.yyyy",
                     "dátum vyhlásenia" : "dd.mm.yyyy",
                     "legislatívny proces" : "LP/yyyy/číslo_procesu",
                     "názov" : "plný_názov_predpisu",
                     "právna oblasť" : "Spotrebné dane",
                     "typ" : "typ_predpisu",
                     "číslo predpisu" : "číslo_predpisu/yyyy Z. z."
                  },
                  revisions => [ #revízie predpisu
                      {
                         'index'=>"číslo_revízie",
                         'uri'=>URI objekt "Slov-Lex URI revízie predpisu",
                         "info" : {
                            "autor" : "Ministerstvo financií Slovenskej republiky",
                            "dátum schválenia" : "dd.mm.yyyy",
                            "dátum vyhlásenia" : "dd.mm.yyyy",
                            "dátum účinnosti do" : "dd.mm.yyyy",
                            "dátum účinnosti od" : "dd.mm.yyyy",
                            "legislatívny proces" : "LP/yyyy/číslo_procesu",
                            "názov" : "plný_názov_predpisu",
                            "právna oblasť" : "Spotrebné dane",
                            "typ" : "typ_predpisu",
                            "číslo predpisu" : "číslo_predpisu/yyyy Z. z."
                         },
                        "structure" : [ #štruktúra revízie predpisu (bude doplnená)
                            {
                               "uri" : "https://static.slov-lex.sk/static/SK/ZZ/2025/1/vyhlasene_znenie.html#predpis.clanok-1"
                            }
                         ]
                      }
                  ]
                  }]
```

### Príklad výstupu ###
```
:~/slov-lex$ perl scrap.pl -since=2023 -till=2023 -cache=./slovlex-cache -proxy=http://proxyhost:port
Processing: 2023. Done
1/2023%Zákon%Zákon, ktorým sa mení a dopĺňa zákon č. 311/2001 Z. z. Zákonník práce v znení neskorších predpisov%https://www.slov-lex.sk/pravne-predpisy/SK/ZZ/2023/1/
2/2023%Zákon%Zákon, ktorým sa mení a dopĺňa zákon č. 220/2004 Z. z. o ochrane a využívaní poľnohospodárskej pôdy a o zmene zákona č. 245/2003 Z. z. o integrovanej prevencii a kontrole znečisťovania životného prostredia a o zmene a doplnení niektorých zákonov v znení neskorších predpisov a ktorým sa menia a dopĺňajú niektoré zákony%https://www.slov-lex.sk/pravne-predpisy/SK/ZZ/2023/2/
3/2023%Nariadenie%Nariadenie vlády Slovenskej republiky, ktorým sa ustanovujú pravidlá poskytovania podpory na neprojektové opatrenia Strategického plánu spoločnej poľnohospodárskej politiky%https://www.slov-lex.sk/pravne-predpisy/SK/ZZ/2023/3/
4/2023%Nariadenie%Nariadenie vlády Slovenskej republiky, ktorým sa mení a dopĺňa nariadenie vlády Slovenskej republiky č. 50/2007 Z. z. o registrácii odrôd pestovaných rastlín v znení neskorších predpisov%https://www.slov-lex.sk/pravne-predpisy/SK/ZZ/2023/4/
5/2023%Nariadenie%Nariadenie vlády Slovenskej republiky, ktorým sa mení a dopĺňa nariadenie vlády Slovenskej republiky č. 195/2018 Z. z., ktorým sa ustanovujú podmienky na poskytnutie investičnej pomoci, maximálna intenzita investičnej pomoci a maximálna výška investičnej pomoci v regiónoch Slovenskej republiky v znení neskorších predpisov%https://www.slov-lex.sk/pravne-predpisy/SK/ZZ/2023/5/
6/2023%Zákon%Zákon, ktorým sa mení a dopĺňa zákon č. 7/2005 Z. z. o konkurze a reštrukturalizácii a o zmene a doplnení niektorých zákonov v znení neskorších predpisov%https://www.slov-lex.sk/pravne-predpisy/SK/ZZ/2023/6/
7/2023%Nariadenie%Nariadenie vlády Slovenskej republiky o výške pracovnej odmeny a podmienkach jej poskytovania odsúdeným%https://www.slov-lex.sk/pravne-predpisy/SK/ZZ/2023/7/
8/2023%Zákon%Zákon, ktorým sa mení a dopĺňa zákon č. 513/1991 Zb. Obchodný zákonník v znení neskorších predpisov a ktorým sa menia a dopĺňajú niektoré zákony%https://www.slov-lex.sk/pravne-predpisy/SK/ZZ/2023/8/
```

### Príklad výstupu JSON ###
```
{
   "2025" : {
      "lexs" : [
         {
            "fullname" : "Vyhláška Ministerstva financií Slovenskej republiky, ktorou sa mení a dopĺňa vyhláška Ministerstva financií Slovenskej r
epubliky č. 537/2011 Z. z., ktorou sa ustanovujú podrobnosti o požiadavkách na usporiadanie výrobného zariadenia na výrobu liehu, technologického 
zariadenia na spracovanie liehu, skladovanie liehu, prepravu liehu, vyskladňovanie liehu a preberanie liehu, kontrole množstva liehu, zisťovaní zá
sob liehu a o spôsobe vedenia evidencie liehu (o kontrole výroby a obehu liehu) v znení vyhlášky č. 82/2013 Z. z.",
            "index" : "1/2025",
            "info" : {
               "autor" : "Ministerstvo financií Slovenskej republiky",
               "dátum schválenia" : "20.11.2024",
               "dátum vyhlásenia" : "08.01.2025",
               "legislatívny proces" : "LP/2024/505",
               "názov" : "Vyhláška Ministerstva financií Slovenskej republiky, ktorou sa mení a dopĺňa vyhláška Ministerstva financií Slovenskej r
epubliky č. 537/2011 Z. z., ktorou sa ustanovujú podrobnosti o požiadavkách na usporiadanie výrobného zariadenia na výrobu liehu, technologického 
zariadenia na spracovanie liehu, skladovanie liehu, prepravu liehu, vyskladňovanie liehu a preberanie liehu, kontrole množstva liehu, zisťovaní zá
sob liehu a o spôsobe vedenia evidencie liehu (o kontrole výroby a obehu liehu) v znení vyhlášky č. 82/2013 Z. z.",
               "právna oblasť" : "Spotrebné dane",
               "typ" : "Vyhláška",
               "číslo predpisu" : "1/2025 Z. z."
            },
            "revisions" : [
               {
                  "index" : "1.",
                  "info" : {
                     "autor" : "Ministerstvo financií Slovenskej republiky",
                     "dátum schválenia" : "20.11.2024",
                     "dátum vyhlásenia" : "08.01.2025",
                     "legislatívny proces" : "LP/2024/505",
                     "názov" : "Vyhláška Ministerstva financií Slovenskej republiky, ktorou sa mení a dopĺňa vyhláška Ministerstva financií Sloven
skej republiky č. 537/2011 Z. z., ktorou sa ustanovujú podrobnosti o požiadavkách na usporiadanie výrobného zariadenia na výrobu liehu, technologického zariadenia na spracovanie liehu, skladovanie liehu, prepravu liehu, vyskladňovanie liehu a preberanie liehu, kontrole množstva liehu, zisťovaní zásob liehu a o spôsobe vedenia evidencie liehu (o kontrole výroby a obehu liehu) v znení vyhlášky č. 82/2013 Z. z.",
                     "právna oblasť" : "Spotrebné dane",
                     "typ" : "Vyhláška",
                     "číslo predpisu" : "1/2025 Z. z."
                  },
                  "structure" : [
                     {
                        "uri" : "https://static.slov-lex.sk/static/SK/ZZ/2025/1/vyhlasene_znenie.html#predpis.clanok-1"
                     }
                  ],
                  "uri" : "https://static.slov-lex.sk/static/SK/ZZ/2025/1/vyhlasene_znenie.html"
               },
               {
                  "index" : "2.",
                  "info" : {
                     "autor" : "Ministerstvo financií Slovenskej republiky",
                     "dátum schválenia" : "20.11.2024",
                     "dátum vyhlásenia" : "08.01.2025",
                     "dátum účinnosti od" : "15.01.2025",
                     "legislatívny proces" : "LP/2024/505",
                     "názov" : "Vyhláška Ministerstva financií Slovenskej republiky, ktorou sa mení a dopĺňa vyhláška Ministerstva financií Slovenskej republiky č. 537/2011 Z. z., ktorou sa ustanovujú podrobnosti o požiadavkách na usporiadanie výrobného zariadenia na výrobu liehu, technologického zariadenia na spracovanie liehu, skladovanie liehu, prepravu liehu, vyskladňovanie liehu a preberanie liehu, kontrole množstva liehu, zisťovaní zásob liehu a o spôsobe vedenia evidencie liehu (o kontrole výroby a obehu liehu) v znení vyhlášky č. 82/2013 Z. z.",
                     "právna oblasť" : "Spotrebné dane",
                     "typ" : "Vyhláška",
                     "číslo predpisu" : "1/2025 Z. z."
                  },
                  "structure" : [
                     {
                        "uri" : "https://static.slov-lex.sk/static/SK/ZZ/2025/1/20250115.html#predpis.clanok-1"
                     }
                  ],
                  "uri" : "https://static.slov-lex.sk/static/SK/ZZ/2025/1/20250115.html"
               }
            ],
            "type" : "Vyhláška",
            "uri" : "https://static.slov-lex.sk/static/SK/ZZ/2025/1/"
         },
         {
            "fullname" : "Oznámenie Ministerstva práce, sociálnych vecí a rodiny Slovenskej republiky o uložení kolektívnej zmluvy vyššieho stupňa uzatvorenej na roky 2024 – 2027 z ؘ27. novembra 2024 medzi Odborovým zväzom KOVO a Združením bytového hospodárstva na Slovensku a Dodatku č. 19 ku Kolektívnej zmluve vyššieho stupňa uzatvorenej na základe rozhodnutia rozhodcu zo dňa 14. marca 2012 medzi Slovenským odborovým zväzom zdravotníctva a sociálnych služieb a Asociáciou štátnych nemocníc Slovenskej republiky",
            "index" : "2/2025",
            "info" : {
               "autor" : "Ministerstvo práce, sociálnych vecí a rodiny Slovenskej republiky",
               "dátum vyhlásenia" : "08.01.2025",
               "názov" : "Oznámenie Ministerstva práce, sociálnych vecí a rodiny Slovenskej republiky o uložení kolektívnej zmluvy vyššieho stupňa uzatvorenej na roky 2024 – 2027 z ؘ27. novembra 2024 medzi Odborovým zväzom KOVO a Združením bytového hospodárstva na Slovensku a Dodatku č. 19 ku Kolektívnej zmluve vyššieho stupňa uzatvorenej na základe rozhodnutia rozhodcu zo dňa 14. marca 2012 medzi Slovenským odborovým zväzom zdravotníctva a sociálnych služieb a Asociáciou štátnych nemocníc Slovenskej republiky",
               "právna oblasť" : "Kolektívne pracovno-právne vzťahy",
               "typ" : "Oznámenie",
               "číslo predpisu" : "2/2025 Z. z."
            },
            "revisions" : [
...
```

### Štatistiky ###
Skript dokáže vypísať aj jednoduché štatistiky o počte predpisov.
```
Years processed: 35
Lexs per year:
  2002 785
  1999 406
  1998 421
  2003 657
  2005 690
  2000 493
  2006 719
  2019 508
  2018 415
  2001 610
  2007 681
  2004 801
  1997 402
  2022 526
  1994 392
  2013 520
  1991 671
  2012 473
  2023 534
  2015 455
  1996 392
  1990 680
  2016 395
  1995 317
  2020 453
  2010 572
  2014 428
  2021 552
  2017 361
  1993 342
  2024 219
  2011 585
  1992 795
  2009 609
  2008 660
```
