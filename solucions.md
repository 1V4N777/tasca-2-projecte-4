# Solucions – Tasca 02: Historial de versions amb Git

## Introducció

En aquesta activitat treballarem amb Git per aprendre a consultar l'historial de versions d'un repositori, revisar els canvis realitzats i recuperar versions anteriors dels fitxers.

També aprendrem a diferenciar entre les ordres `git restore`, `git revert` i `git reset`, que permeten gestionar els canvis del projecte de diferents maneres.

---

# Part 1. Bones pràctiques en la documentació de commits

## Exercici 1. Crear commits estructurats

Primer vaig crear un repositori anomenat `projecte4-tasca02` i vaig inicialitzar el projecte amb un fitxer `README.md`.

Aquest fitxer conté una breu descripció de la funcionalitat del repositori.

Després vaig fer el primer commit seguint la convenció de missatges:

```bash
git commit -m "docs: inicialitza la repositori de prova per tasca 2"
```

A continuació, vaig crear el fitxer `00-installer-config.yaml` amb la configuració inicial de xarxa mitjançant DHCP per a la interfície `enps03`.

Contingut del fitxer:

```yaml
network:
  version: 2
  renderer: networkd
  ethernets:
    enps03:
      dhcp4: true
```

Vaig guardar els canvis amb un commit de tipus `feat`:

```text
feat: afegeix configuracio DHCP per a enps03
```

**Conclusió:** Els commits han de tenir missatges clars i descriptius per facilitar la comprensió dels canvis realitzats.

---

# Part 2. L'historial del repositori

## Exercici 1. Consultar l'historial detallat

Per consultar l'historial complet del repositori vaig executar:

```bash
git log
```

Aquesta ordre mostra informació de cada commit, com ara:
- El hash identificador.
- L'autor del commit.
- La data de realització.
- El missatge descriptiu.

## Exercici 2. Consultar l'historial resumit

Després vaig executar:

```bash
git log --oneline
```

Aquesta ordre mostra una versió més resumida de l'historial, amb el hash abreujat i el missatge de cada commit.

És més ràpida per identificar els commits realitzats.

## Exercici 3. Consultar l'historial amb Git Graph

Vaig utilitzar l'extensió Git Graph de Visual Studio Code per visualitzar gràficament l'historial del repositori.

Aquesta eina permet veure els commits, les branques i la seva relació cronològica.

## Exercici 4. Inspeccionar els canvis de l'últim commit

Per consultar els canvis introduïts en l'últim commit vaig executar:

```bash
git show HEAD
```

Aquesta ordre mostra el commit seleccionat i les diferències respecte a la versió anterior del fitxer.

També vaig consultar aquesta informació des de Git Graph.

---

# Part 3. Recuperar versions anteriors del Working Directory

## Exercici 1. Simular un error humà

Per simular un error, vaig modificar el fitxer `00-installer-config.yaml` i vaig canviar el nom de la interfície de `enps03` a `eth0`.

La configuració modificada era:

```yaml
network:
  version: 2
  renderer: networkd
  ethernets:
    eth0:
      dhcp4: true
```

Això simula una modificació incorrecta del fitxer.

## Exercici 2. Comprovar l'estat del repositori

Vaig executar:

```bash
git status
```

Git va indicar que el fitxer `00-installer-config.yaml` havia estat modificat.

A Visual Studio Code també es podia observar la lletra `M`, que indica que el fitxer té modificacions respecte a l'últim commit.

## Exercici 3. Descartar els canvis

Per recuperar la versió anterior del fitxer vaig executar:

```bash
git restore 00-installer-config.yaml
```

Aquesta ordre descarta les modificacions del Working Directory i recupera el contingut de l'últim commit.

Després vaig tornar a comprovar l'estat del repositori:

```bash
git status
```

El resultat va indicar que no hi havia canvis pendents.

**Conclusió:** `git restore` permet recuperar la versió guardada en l'últim commit quan encara no hem creat un nou commit amb els canvis.

---

# Part 4. Recuperar versions anteriors (commits) del repositori

## Exercici 1. Configurar una IP estàtica

Primer vaig modificar el fitxer `00-installer-config.yaml` per desactivar DHCP i configurar una IP estàtica a la interfície `enps03`.

La configuració utilitzada va ser:

```yaml
network:
  version: 2
  renderer: networkd
  ethernets:
    enps03:
      dhcp4: false
      addresses: [192.168.1.100/24]
      routes:
        - to: default
          via: 192.168.1.1
      nameservers:
        addresses: [192.168.1.1]
```

Vaig guardar els canvis amb el commit:

```text
feat: configura IP estatica per a enps03
```

## Exercici 2. Afegir una segona interfície

Després vaig modificar la configuració per afegir una segona interfície anomenada `enps08`, configurada amb DHCP.

El fragment afegit va ser:

```yaml
    enps08:
      dhcp4: true
```

Vaig guardar aquesta modificació mitjançant un nou commit.

## Exercici 3. Modificar el servidor DNS

Finalment, vaig modificar el servidor DNS de la interfície `enps03`, substituint l'adreça anterior per `8.8.8.8`.

La configuració va quedar així:

```yaml
nameservers:
  addresses: [8.8.8.8]
```

Vaig guardar el canvi amb un commit descriptiu:

```text
fix: actualitza el servidor DNS de enps03
```

## Exercici 4. Revisar l'historial de commits

Per comprovar els canvis realitzats vaig executar:

```bash
git log --oneline
```

També vaig utilitzar Git Graph per visualitzar els commits i comprovar-ne l'ordre.

## Exercici 5. Recuperar una versió anterior amb git revert

L'objectiu era tornar a l'estat del segon commit, que contenia la configuració inicial amb DHCP per a la interfície `enps03`.

Vaig provar de fer el procés amb Git Graph i l'ordre `git revert`.

Durant el procés van aparèixer conflictes relacionats amb els fitxers d'imatge del repositori. Per això, vaig haver de cancel·lar un intent de revert amb:

```bash
git revert --abort
```

També es van generar commits de tipus `Revert` durant els intents posteriors.

Per recuperar la configuració DHCP inicial, vaig editar manualment el fitxer `00-installer-config.yaml` i vaig deixar la configuració següent:

```yaml
network:
  version: 2
  renderer: networkd
  ethernets:
    enps03:
      dhcp4: true
```

Aquesta configuració recupera l'estat inicial de DHCP.

## Exercici 6. Explorar un commit anterior

Per inspeccionar els canvis d'un commit anterior vaig utilitzar Git Graph i vaig seleccionar el commit:

```text
4a2d4e6 feat: configura IP estatica per a enps03
```

També vaig consultar els detalls des de la terminal amb:

```bash
git show 4a2d4e6293ddc3152180b045afc74dd5999cc7a3
```

El resultat mostrava les diferències introduïdes en el fitxer `00-installer-config.yaml`:

- DHCP passava de `true` a `false`.
- S'afegia l'adreça IP estàtica `192.168.1.100/24`.
- S'afegia la porta d'enllaç `192.168.1.1`.
- S'afegia el servidor DNS `192.168.1.1`.

També es podia observar que s'havia afegit una captura a la carpeta `img`.

Les diferències permeten identificar les línies afegides en verd i les eliminades en vermell.

## Exercici 7. Eliminar l'últim commit amb git reset

Per acabar, vaig consultar l'historial abans d'eliminar l'últim commit:

```bash
git log --oneline
```

Abans de fer el reset vaig comprovar l'estat del repositori amb:

```bash
git status
```

Després vaig executar:

```bash
git reset --hard HEAD~1
```

Aquesta ordre elimina l'últim commit de la branca actual i situa `HEAD` en el commit anterior. També restaura el Working Directory i l'àrea de preparació a l'estat d'aquest commit.

El resultat de la terminal va ser:

```text
HEAD is now at ca36401 12 img
```

Això indica que el repositori ha tornat al commit `ca36401`.

Finalment, vaig comprovar l'estat del repositori:

```bash
git status
```

El resultat va ser:

```text
On branch main
Your branch is up to date with 'origin/main'.

nothing to commit, working tree clean
```

I vaig tornar a consultar l'historial:

```bash
git log --oneline
```

Vaig comprovar que l'últim commit eliminat ja no apareixia en l'historial.

---

# Captures de pantalla

Totes les captures de pantalla realitzades durant el desenvolupament de l'activitat, que mostren els diferents passos, ordres i resultats obtinguts, es troben guardades a la carpeta `img/` del repositori.

---

# Conclusió final

En aquesta activitat he après a gestionar l'historial de versions d'un repositori amb Git i a recuperar versions anteriors dels fitxers.

He treballat amb diferents ordres, com `git log`, `git show`, `git status`, `git restore`, `git revert` i `git reset`.

He après que `git restore` serveix per descartar modificacions que encara no s'han guardat en un commit, mentre que `git revert` crea un nou commit que desfà els canvis d'un commit anterior.

En canvi, `git reset --hard` permet retrocedir en l'historial eliminant el darrer commit de la branca i descartant els canvis del Working Directory.

També he après la importància de documentar correctament els commits i de revisar l'historial abans de fer operacions que poden eliminar informació.

Aquestes eines són fonamentals per treballar de manera ordenada i segura amb Git i GitHub.