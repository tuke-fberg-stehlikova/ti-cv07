# cv07: PDO, zápis a výpis

Stránka týždňa: https://beata.stehlikova.website.tuke.sk/ti/08.html
Prijať zadanie: https://classroom50.org/tuke-fberg-stehlikova/technologie-internetu-5k/assignments/cv07/accept?k=tcc8jmx0

Výsledkom je aplikácia: každá stránka v menu niekam vedie a všetko číta z tvojej databázy. ENTITA = tvoja main entity.

**Odovzdávaš** (v repository a vo svojom webovom priestore na sigma.tuke.sk v `ti/cv07/`, obsah `web/` aj v `ti/projekt/`):

- `sql/schema.sql`, `sql/seed.sql`, `sql/queries.sql` (z cv06)
- `web/db.php`
- `web/data.php` + `web/content/data.php` (Databáza: výpis všetkých tables so SQL, formuláre na lookup tables)
- `web/add-ENTITA.php` + `web/content/add-ENTITA.php` (formulár na main entity s pivot table)
- stránka so zoznamom z databázy s filtrom `method="get"` a odkazom na detail
- `web/detail.php` + `web/content/detail.php`
- `web/stats.php` + `web/content/stats.php` (Prehľad)
- `web/template.php` s menu podľa stránok aplikácie

**Hodnotenie:** 1 bod. Aplikácia so zoznamom z databázy a filtrom, detailom podľa id, formulárom na main entity s pivot table, stránkou Databáza s formulármi na lookup tables a stránkou Prehľad funguje vo svojom webovom priestore na sigma.tuke.sk, kód je v repository. Po push sa spustia testy (25 kontrol); zelený výsledok znamená, že files a požadované časti kódu sú na mieste, body udeľuje učiteľka.

Ak v Kubeflow chýbajú rozšírenia, príkaz na obnovu je v návode 04 Odovzdávanie.

## Prompty

Tri malé prompty. Ku každému priložíš svoj `sql/schema.sql`, aby AI videla presné názvy tables a columns, a svoj `template.php`, aby stránky sedeli do aplikácie.

Prompt A, pripojenie a stránka Databáza:

```text
Mám PHP web, kde každá stránka je file v web/ s premennými $title, $active, $content a include 'template.php' (priložený), obsah je v content/. Databáza MariaDB má tables podľa priloženého schema.sql. Napíš: 1. web/db.php s pripojením cez PDO, nastavenia ERRMODE_EXCEPTION a FETCH_ASSOC, DSN s charset=utf8mb4, na začiatku ini_set('display_errors', 1) a error_reporting(E_ALL), prihlasovacie údaje nechaj ako // DOPLŇ. 2. web/data.php (require 'db.php' pred include) a web/content/data.php: hore pre každú lookup table malý Bootstrap formulár na pridanie record (method POST, hidden input form s názvom table, hodnoty z $_POST do premenných, kontrola povinných, SELECT ... WHERE name = ? s fetch na kontrolu duplicity, potom INSERT cez prepare a execute s otáznikmi, hláška v alert); pod tým výpis každej table zo schema.sql tak, ako je (SELECT * cez query, SQL vypísané v pre nad tabuľkou, hlavička z array_keys prvého record, hodnoty cez htmlspecialchars). Žiadne funkcie ani triedy, PHP hore a HTML dole s foreach/endforeach, komentár pri každom riadku po slovensky.
```

Prompt B, Nová cesta (zápis do main entity s pivot table):

```text
Pre ten istý web (template.php a schema.sql priložené) napíš web/add-ENTITA.php a web/content/add-ENTITA.php: Bootstrap formulár method POST na pridanie record do main entity ENTITA. Premenná $pdo je pripravená v db.php. Pre foreign key na lookup table select naplnený z databázy (value = id, text = name), pre pivot table checkboxy name="X[]" naplnené z databázy. Po odoslaní: hodnoty z $_POST do premenných, kontrola povinných inputs, INSERT do main entity cez prepare a execute s otáznikmi, lastInsertId, potom INSERT do pivot table vo foreach cez jeden pripravený príkaz. Hláška v alert s počtom zaškrtnutých položiek. Validačné pravidlá nechaj ako // DOPLŇ. Bez funkcií a tried, komentár pri každom riadku.
```

Prompt C, zoznam z databázy, detail a prehľad:

```text
Pre ten istý web (template.php, schema.sql a priložený content/ZOZNAM.php so statickými kartami): 1. prerob content/ZOZNAM.php tak, aby karty čítal z databázy cez PDO: SELECT s JOIN na lookup table, zoradené od najnovšieho; nad zoznam formulár method GET so selectom podľa lookup table a možnosťou všetky; keď je parameter v URL, prepare s WHERE ... = ?, inak query; HTML kariet zachovaj, zmeň len zdroj dát; pri každej karte odkaz detail.php?id=... 2. web/detail.php a content/detail.php: id z $_GET, jeden record cez prepare a fetch, keď neexistuje hláška a return; položky pivot table tohto record druhým SELECT cez prepare. 3. web/stats.php a content/stats.php: tabuľky main entity s hodnotami z lookup table, pivot table s názvami z oboch strán, počet podľa lookup table (GROUP BY), jeden record naj (ORDER BY ... LIMIT 1, fetch); SQL v premenných a vypísané v pre nad každou tabuľkou. Výstup cez htmlspecialchars, Bootstrap table a card, komentár pri každom zmenenom riadku.
```

## Úlohy

**8.1** Kubeflow: skontroluj rozšírenia (SFTP, Five Server, Intelephense). Ak chýbajú, spusti v termináli príkaz na obnovu z návodu [04 Odovzdávanie](https://beata.stehlikova.website.tuke.sk/navody/04-odovzdavanie.html).

**8.2** `git clone` repository cv07. Prekopíruj `web/` z cv05 (cv06 obsahoval len SQL) a SQL files z cv06:

```bash
rsync -a --exclude .git cv05/ cv07/
mkdir -p cv07/sql
cp cv06/*.sql cv07/sql/
```

V Adminer spusti `schema.sql` a potom `seed.sql`, aby si začínal z čistých dát. Priečinok `practice/` tento týždeň nepotrebuješ, môžeš ho nechať tak.

**8.3** `web/db.php`: pripojenie so svojimi údajmi z cv06 (prompt A alebo ukážka 1). Over v prehliadači: prázdna stránka bez hlášky.

**8.4** Stránka Databáza (`web/data.php` + `content/data.php`): výpis každej svojej table tak, ako je, s id a foreign keys, so SQL nad tabuľkou (prompt A alebo ukážka 2). Pridaj ju do menu v `template.php`.

**8.5** Na stránku Databáza doplň formulár na pridanie record pre každú svoju lookup table (prepare, execute, kontrola duplicity cez `fetch`). Skús pridať hodnotu, ktorá už existuje, a do komentára v `content/data.php` napíš, čo by databáza vrátila bez tvojej kontroly a prečo.

**8.6** Stránka Nová ENTITA (`web/add-ENTITA.php` + `content/add-ENTITA.php`): Bootstrap formulár na pridanie record do main entity vrátane pivot table (prompt B alebo ukážka 3). Select a checkboxy sa plnia z databázy. Do komentára na začiatok file napíš, koľko INSERT príkazov sa pošle pri dvoch zaškrtnutých checkboxoch. Over na stránke Databáza, že record je v main entity aj dvojice v pivot table.

**8.7** Stránka so zoznamom (vo vzore `content/routes.php`): karty z databázy cez JOIN, filter `method="get"` podľa jednej lookup table, tlačidlo na stránku Nová ENTITA (prompt C alebo ukážka 4). Statické dáta z týždňov 4 až 6 zo súboru zmaž. Over, že po výbere sa zmení URL aj výpis.

**8.8** Detail (`web/detail.php` + `content/detail.php`): jeden record podľa `id` z URL cez prepare a fetch, odkaz „detail" pri každej karte, položky pivot table druhým SELECT (ukážka 5). Zlú verziu nechaj v komentári a do komentára napíš URL, ktorá by v zlej verzii vrátila všetko.

**8.9** Stránka Prehľad (`web/stats.php` + `content/stats.php`): main entity s hodnotami z lookup table (JOIN), pivot table s názvami z oboch strán, počet records podľa lookup table (`GROUP BY`), jeden record „naj" (`fetch`); SQL vypísané nad každou tabuľkou (prompt C alebo ukážka 6). Pridaj do menu.

**8.10** Menu v `template.php` zodpovedá stránkam aplikácie: Domov, zoznam, Nová ENTITA, Databáza, Prehľad, Kontakt. Registračný formulár z týždňa 6 z menu odstráň; file nechaj v repository, vráti sa v nepovinnom cv09.

**8.11** Commit a push, aspoň 4 commity v rôznych krokoch (napr. po 8.4, 8.6, 8.7, 8.9).

**8.12** Upload Folder: `cv07` do `ti/cv07/` a obsah `web/` do `ti/projekt/`. Over vo svojom webovom priestore na sigma.tuke.sk: každá položka menu funguje, zoznam číta z databázy, filter mení URL aj výpis, formuláre zapisujú, detail sa otvára podľa id.

---

# cv07: PDO, insert and select

Week page: https://beata.stehlikova.website.tuke.sk/ti/08.html
Accept the assignment: https://classroom50.org/tuke-fberg-stehlikova/technologie-internetu-5k/assignments/cv07/accept?k=tcc8jmx0

The result is an application: every menu item leads somewhere and everything reads from your database. ENTITY = your main entity.

**You submit** (in the repository and in your web space on sigma.tuke.sk in `ti/cv07/`, the content of `web/` also in `ti/projekt/`):

- `sql/schema.sql`, `sql/seed.sql`, `sql/queries.sql` (from cv06)
- `web/db.php`
- `web/data.php` + `web/content/data.php` (Database: all tables listed with the SQL, forms for the lookup tables)
- `web/add-ENTITY.php` + `web/content/add-ENTITY.php` (the form for the main entity with the pivot table)
- the list page from the database with a `method="get"` filter and a detail link
- `web/detail.php` + `web/content/detail.php`
- `web/stats.php` + `web/content/stats.php` (Overview)
- `web/template.php` with the menu matching the application pages

**Grading:** 1 point. The application with the list from the database and a filter, the detail by id, the form for the main entity with the pivot table, the Database page with forms for the lookup tables and the Overview page works in your web space on sigma.tuke.sk, and the code is in the repository. Tests run after the push (25 checks); a green result means the files and the required parts of the code are in place, the points are awarded by the teacher.

If extensions are missing in Kubeflow, the restore command is in guide 04 Submitting.

## Prompts

Three small prompts. Attach your `sql/schema.sql` to each, so that the AI sees the exact table and column names, and your `template.php`, so that the pages fit the application.

Prompt A, connection and the Database page:

```text
I have a PHP site where every page is a file in web/ with the variables $title, $active, $content and include 'template.php' (attached); the content is in content/. The MariaDB database has the tables in the attached schema.sql. Write: 1. web/db.php with a PDO connection, settings ERRMODE_EXCEPTION and FETCH_ASSOC, a DSN with charset=utf8mb4, ini_set('display_errors', 1) and error_reporting(E_ALL) at the top, login details left as // TODO. 2. web/data.php (require 'db.php' before the include) and web/content/data.php: at the top, for every lookup table a small Bootstrap form to add a record (method POST, a hidden input form with the table name, values from $_POST into variables, a check of required inputs, SELECT ... WHERE name = ? with fetch to check duplicates, then INSERT through prepare and execute with question marks, a message in an alert); below it a listing of every table from schema.sql as it is (SELECT * through query, the SQL printed in a pre above the table, the header from array_keys of the first record, values through htmlspecialchars). No functions or classes, PHP at the top and HTML below with foreach/endforeach, a comment on every line.
```

Prompt B, New route (insert into the main entity with the pivot table):

```text
For the same site (template.php and schema.sql attached) write web/add-ENTITY.php and web/content/add-ENTITY.php: a Bootstrap form, method POST, to add a record to the main entity ENTITY. The variable $pdo is prepared in db.php. For the foreign key to a lookup table a select filled from the database (value = id, text = name), for the pivot table checkboxes name="X[]" filled from the database. After submitting: values from $_POST into variables, a check of required inputs, INSERT into the main entity through prepare and execute with question marks, lastInsertId, then INSERT into the pivot table in a foreach through one prepared statement. A message in an alert with the number of checked items. Leave the validation rules as // TODO. No functions or classes, a comment on every line.
```

Prompt C, the list from the database, the detail and the overview:

```text
For the same site (template.php, schema.sql and the attached content/LIST.php with static cards): 1. rework content/LIST.php so the cards are read from the database through PDO: SELECT with a JOIN to the lookup table, newest first; above the list a form method GET with a select by the lookup table and an option all; when the parameter is in the URL use prepare with WHERE ... = ?, otherwise query; keep the card HTML, change only the data source; on every card a link detail.php?id=... 2. web/detail.php and content/detail.php: id from $_GET, one record through prepare and fetch, a message and return when it does not exist; the pivot table items of this record with a second SELECT through prepare. 3. web/stats.php and content/stats.php: tables of the main entity with values from the lookup table, the pivot table with names from both sides, counts per lookup table (GROUP BY), one "most" record (ORDER BY ... LIMIT 1, fetch); the SQL in variables and printed in a pre above every table. Output through htmlspecialchars, Bootstrap table and card, a comment on every changed line.
```

## Tasks

**8.1** Kubeflow: check the extensions (SFTP, Five Server, Intelephense). If they are missing, run the restore command from guide [04 Submitting](https://beata.stehlikova.website.tuke.sk/navody/04-odovzdavanie.html) in the terminal.

**8.2** `git clone` repository cv07. Copy `web/` from cv05 (cv06 held only SQL) and the SQL files from cv06:

```bash
rsync -a --exclude .git cv05/ cv07/
mkdir -p cv07/sql
cp cv06/*.sql cv07/sql/
```

In Adminer run `schema.sql` and then `seed.sql`, so that you start from clean data. You do not need the `practice/` folder this week, you can leave it as it is.

**8.3** `web/db.php`: the connection with your details from cv06 (prompt A or example 1). Check in the browser: a blank page without a message.

**8.4** The Database page (`web/data.php` + `content/data.php`): a listing of every table of yours as it is, with ids and foreign keys, with the SQL above the table (prompt A or example 2). Add it to the menu in `template.php`.

**8.5** On the Database page add a form to add a record for every lookup table of yours (prepare, execute, a duplicate check with `fetch`). Try adding a value that already exists and write in a comment in `content/data.php` what the database would return without your check and why.

**8.6** The New ENTITY page (`web/add-ENTITY.php` + `content/add-ENTITY.php`): a Bootstrap form that adds a record to the main entity including the pivot table (prompt B or example 3). The select and the checkboxes are filled from the database. In a comment at the top of the file write how many INSERT statements are sent with two checked checkboxes. Check on the Database page that the record is in the main entity and the pairs in the pivot table.

**8.7** The list page (in the sample `content/routes.php`): cards from the database through a JOIN, a `method="get"` filter by one lookup table, a button to the New ENTITY page (prompt C or example 4). Delete the static data from weeks 4 to 6 from the file. Check that after choosing, both the URL and the listing change.

**8.8** The detail (`web/detail.php` + `content/detail.php`): one record by `id` from the URL through prepare and fetch, a "detail" link on every card, the pivot table items with a second SELECT (example 5). Leave the bad version in a comment and write in a comment the URL that would return everything in the bad version.

**8.9** The Overview page (`web/stats.php` + `content/stats.php`): the main entity with values from the lookup table (JOIN), the pivot table with names from both sides, the number of records per lookup table (`GROUP BY`), one "most" record (`fetch`); the SQL printed above every table (prompt C or example 6). Add it to the menu.

**8.10** The menu in `template.php` matches the pages of the application: Home, the list, New ENTITY, Database, Overview, Contact. Remove the week 6 registration form from the menu; keep the file in the repository, it returns in the optional cv09.

**8.11** Commit and push, at least 4 commits at different steps (e.g. after 8.4, 8.6, 8.7, 8.9).

**8.12** Upload Folder: `cv07` into `ti/cv07/` and the content of `web/` into `ti/projekt/`. Check in your web space on sigma.tuke.sk: every menu item works, the list reads from the database, the filter changes the URL and the listing, the forms write, the detail opens by id.
