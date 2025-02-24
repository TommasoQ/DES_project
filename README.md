# Progetto DES – Sistemi a Eventi Discreti (A.A. 2023/24)

Questo repository contiene il materiale sviluppato da Brogi Andrea, Burroni Leonardo, Caproni Edoardo, Quintabà Tommaso per il progetto di Sistemi a Eventi Discreti. Il progetto si focalizza sulla modellizzazione e simulazione di un sistema produttivo costituito da un magazzino (B) e da due macchine in serie (M1 e M2), come descritto nel testo dell'esercitazione.

## Indice

- [Introduzione](#introduzione)
- [Descrizione del Sistema](#descrizione-del-sistema)
- [Modellizzazione e Metodologia](#modellizzazione-e-metodologia)
  - [Punto 1: Modello Logico](#punto-1-modello-logico)
  - [Punto 2: Scelta e Dimensionamento dei Parametri](#punto-2-scelta-e-dimensionamento-dei-parametri)
  - [Punto 3: Stima delle Probabilità Limite](#punto-3-stima-delle-probabilità-limite)
  - [Punto 4: Distribuzione del Tempo di Attesa](#punto-4-distribuzione-del-tempo-di-attesa)
  - [Punto 5: Stima di λeff e μeff](#punto-5-stima-di-λeff-e-μeff)
  - [Punto 6: Verifica della Legge di Little per il Sottosistema Σ](#punto-6-verifica-della-legge-di-little-per-il-sottosistema-Σ)
  - [Punto 7: Analisi della Steady State tramite Distribuzioni Empiriche](#punto-7-analisi-della-steady-state-tramite-distribuzioni-empiriche)
  - [Punto 8: Ripetizione delle Stime con Dati Non Esponenziali](#punto-8-ripetizione-delle-stime-con-dati-non-esponenziali)
- [Contributori](#contributori)

## Introduzione

Il progetto mira a studiare il comportamento di un sistema produttivo attraverso metodi di simulazione e analisi teorica. In particolare, si analizzano le prestazioni del sistema in regime, stimando le probabilità limite degli stati, il tempo di attesa nel buffer, i ratei di ingresso/uscita e altre misure di performance, confrontando i risultati ottenuti in simulazione con quelli analitici.

## Descrizione del Sistema

Il sistema considerato è costituito da:
- **Magazzino B:** capacità unitaria, dove le parti possono essere immagazzinate.
- **Macchina M1 e Macchina M2:** interconnesse in serie.
  - Le parti in arrivo seguono un processo di Poisson.
  - Le durate di lavorazione in M1 e M2 sono modellate da distribuzioni esponenziali.
  - Una frazione \( f = \frac{4}{9} \) delle parti richiede la lavorazione esclusivamente in M2, mentre le altre devono essere processate in entrambe le macchine.
  - In caso di magazzino pieno, le parti vengono respinte.
  - Se M1 termina un lavoro e M2 è ancora occupata, M1 trattiene il pezzo finché M2 non diventa disponibile; le parti provenienti da M1 hanno priorità su quelle in attesa nel buffer.

## Modellizzazione e Metodologia

### Punto 1: Modello Logico

Il sistema è stato modellato come un automa a stati stocastico, definito dall'insieme:
- \( S = \{ E, X, \Gamma, p, x_0, F \} \)
- **Eventi (E):** \( \{ a, d_1, d_2 \} \)  
  L'evento \( a \) può portare, con probabilità \( f = \frac{4}{9} \), all’aggiunta di un pezzo da processare solo in M2 oppure, con probabilità \( 1-f = \frac{5}{9} \), all’aggiunta in entrambi i reparti.
- **Stati (X):** Componenti che rappresentano la situazione del buffer e lo stato delle macchine.
- **Stati Finali (F):** Con associati tempi di vita esponenziali \( V_a \), \( V_1 \), \( V_2 \) per gli eventi, rispettivamente con parametri \( \lambda_a \), \( \mu_1 \), \( \mu_2 \).

### Punto 2: Scelta e Dimensionamento dei Parametri

Per rispettare i requisiti dell'esercitazione (tempo di assestamento \( t_\delta \) compreso tra 35 e 45 minuti e probabilità limite non inferiori a \( 10^{-2} \)), sono stati sperimentati vari set di parametri:
- Inizialmente si è provato con \( (\lambda_a, \mu_1, \mu_2) = (1, 1, 1) \), ma i valori risultanti erano troppo elevati.
- Sono stati testati ulteriori set, tra cui:
  - \( (0.1, 0.1, 0.1) \)
  - \( (0.7, 0.3, 0.3) \)
  - \( (0.4, 0.3, 0.3) \)
  - \( (0.4, 0.2, 0.2) \)

### Punto 3: Stima delle Probabilità Limite

Si è simulato il sistema fino a 100 eventi per garantire l’avvicinamento al regime. A 40 minuti di simulazione (considerati sufficienti per raggiungere lo steady state), le probabilità degli stati sono state stimate mediando più osservazioni (con \( n = 10, 100, 1000, 10000 \) campioni). Le tabelle riportano i valori medi e le varianze delle stime, evidenziando l’effetto della legge dei grandi numeri.

### Punto 4: Distribuzione del Tempo di Attesa

È stata stimata la distribuzione del tempo di attesa nel buffer per il quinto cliente ammesso, verificando che il numero di eventi simulati (circa 100) garantisca la completa esecuzione dell’ingresso e uscita del cliente.

### Punto 5: Stima di λeff e μeff

Sia in modo empirico (contando il numero di arrivi/partenze in regime) sia analitico (usando le probabilità limite degli stati) sono stati stimati:
- \( \lambda_{\text{eff}} \approx 0.1511 \) (stimato) e \( 0.1513 \) (analitico)
- \( \mu_{\text{eff}} \approx 0.1511 \) (stimato) e \( 0.1513 \) (analitico)

L’errore tra le due misure è inferiore a \( 10^{-3} \).

### Punto 6: Verifica della Legge di Little per il Sottosistema Σ

Per il sottosistema costituito dalla sola macchina M1, sono state stimate:
- Tempo medio di sistema \( E[S_{\Sigma}] \)
- Occupazione media \( E[X_{\Sigma}] \)
- Tasso di ingresso \( \lambda_{\Sigma} \)

I risultati ottenuti sono stati confrontati con i calcoli analitici, verificando la validità della legge di Little con errore non superiore a \( 10^{-2} \).

### Punto 7: Analisi della Steady State tramite Distribuzioni Empiriche

Utilizzando le distribuzioni empiriche dei lifetimes (ottenute dai dati misurati nel file `gruppo03.mat`), è stata estratta la CDF e, tramite il metodo inverso, sono stati generati nuovi campioni. Successivamente, l’evoluzione temporale delle medie delle probabilità degli stati è stata analizzata per confermare il raggiungimento dello steady state.

### Punto 8: Ripetizione delle Stime con Dati Non Esponenziali

Con dati le cui distribuzioni non sono esponenziali, e a partire da un nuovo istante di regime (600 minuti), sono state ripetute le stime di:
- \( \lambda_{\text{eff}} \)
- \( \lambda_{\Sigma} \)
- Tempo medio di sistema e occupazione
I risultati hanno mostrato una coerenza tra stime empiriche e analitiche.


## Contributori

- **Brogi Andrea**
- **Burroni Leonardo**
- **Caproni Edoardo**
- **Quintabà Tommaso**
