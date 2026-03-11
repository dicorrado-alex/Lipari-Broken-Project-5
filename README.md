# 🔧 LipariBank Broken Project — Day 5

Progetto Java 17+ con **Maven** e **H2**. Simula un repository Git reale con
**3 problemi di workflow Git** che impediscono la corretta collaborazione.
Il codice è compilabile solo **dopo aver risolto i bug**.

---

## Struttura del progetto

```
liparibank-workspace/
├── .git/
├── .gitignore
├── .idea/
│   ├── workspace.xml
│   ├── misc.xml
│   └── encodings.xml
├── out/
│   └── production/liparibank-broken-day5/.gitkeep
├── liparibank.db
├── pom.xml
└── src/main/java/com/lipari/bank/
    ├── model/
    │   ├── Customer.java
    │   └── Account.java
    ├── persistence/
    │   └── DatabaseManager.java
    └── cli/
        └── BankConsole.java
```

---

## Prerequisiti

- **Java 17+** (o 21)
- **Maven 3.6+**
- **Git**

---

## Setup

```bash
cd liparibank-workspace
git log --oneline          # visualizza la storia dei commit
git status                 # stato attuale del repository
mvn compile                # ← non compila! (BUG #2 attivo)
```

---

## 🕵️ Le tue 3 missioni

---

### MISSIONE 1 — I file IDE e il database appaiono sempre come tracciati

**Sintomo:** Il file `.gitignore` contiene le regole giuste (`.idea/`, `*.db`, `out/`),
ma quei file continuano ad apparire come "tracked" (`git ls-files` li mostra).
Modificare `.gitignore` non ha nessun effetto su di loro: sono comunque
inclusi nel repository e visibili a tutti i collaboratori.

---

### MISSIONE 2 — Il progetto non compila: errore su caratteri illegali `<` `>`

**Sintomo:** Eseguendo `mvn compile` il compilatore Java segnala errori su
caratteri illegali nel file `BankConsole.java`. Il metodo `printReport()`
sembra contenere due implementazioni sovrapposte separate da sequenze strane
di caratteri (`<<<<<<<`, `=======`, `>>>>>>>`).

---

### MISSIONE 3 — L'applicazione non si avvia: errore di connessione al database

**Sintomo:** Dopo aver risolto il BUG #2, compilare ed eseguire il progetto
lancia un'eccezione di connessione: il database H2 non riesce ad aprire
il file specificato. Il percorso contiene il nome utente di un'altra persona
e non esiste sulla tua macchina.

---

## ✅ Obiettivo finale

Quando hai trovato e corretto tutti e 3 i bug, riesegui il progetto:

- `git ls-files | grep -E "\.idea|\.db|out/"` deve restituire **output vuoto**
- `mvn compile` deve terminare con **BUILD SUCCESS**
- `mvn compile exec:java -Dexec.mainClass="com.lipari.bank.cli.BankConsole"` deve
  avviare l'applicazione e stampare il report clienti
