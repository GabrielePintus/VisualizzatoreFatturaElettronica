# Visualizzatore Fattura Elettronica

Webapp **single-file** per aprire e leggere fatture elettroniche italiane in formato XML (FatturaPA) direttamente nel browser, **senza backend**, **senza server** e **senza che i dati escano dal tuo PC**.

---

## Privacy garantita

I dati della fattura non lasciano mai il tuo dispositivo.
Il file XML viene letto tramite le API native del browser (`FileReader`, `DOMParser`) e processato interamente in locale. Non viene effettuata nessuna richiesta di rete.

---

## Funzionalità

- **Apertura file** tramite drag & drop o selettore file
- **Incolla XML** direttamente dal testo (utile se il browser blocca l'accesso ai file locali)
- **Visualizzazione completa** di:
  - Dati del documento (tipo, numero, data, totale, bollo virtuale, causale, Art.73)
  - Fornitore e cliente (ragione sociale / nome e cognome, P.IVA, CF, sede, regime fiscale)
  - Dettaglio linee con sconti/maggiorazioni, periodo di competenza, ritenute
  - Riepilogo IVA con natura, esigibilità e riferimento normativo
  - Dati di pagamento con IBAN formattato, BIC, istituto finanziario, scadenza
  - Allegati con download diretto (decodifica base64 in-browser)
- **Lotto fatture** (più `FatturaElettronicaBody` nello stesso file): navigazione a tab
- **Stampa / Salva PDF** tramite la funzione di stampa del browser
- Decodifica di tutti i codici standard: `TipoDocumento` (TD01–TD29), `RegimeFiscale` (RF01–RF20), `ModalitaPagamento` (MP01–MP23), `Natura IVA` (N1–N7.x), `EsigibilitaIVA`, `CondizioniPagamento`
- Gestione sia di persone fisiche (Nome + Cognome) che persone giuridiche (Denominazione)

---

## Formati supportati

| Formato | Descrizione |
|---------|-------------|
| `FPR12` | Fattura tra privati (B2B / B2C) |
| `FPA12` | Fattura verso Pubblica Amministrazione |
| `FSM10` | Fattura semplificata |

Il formato di trasmissione viene rilevato automaticamente dall'attributo `versione` o `FormatoTrasmissione`.

---

## Come usare

1. Apri `index.html` nel browser (Chrome, Firefox, Edge, Safari)
2. Trascina il file `.xml` nella zona di caricamento, oppure clicca **"Scegli file XML"**
3. In alternativa, apri il file `.xml` con un editor di testo, copia tutto il contenuto (Ctrl+A → Ctrl+C) e incollalo tramite il pannello **"incolla il testo XML"**

> **Nota:** Chrome aperto su `file://` può bloccare il drag & drop su alcune configurazioni. In quel caso usa il selettore file o il pannello di incolla.

---

## Limitazioni

- **File `.p7m` (firma CAdES-BES):** i file firmati digitalmente con estensione `.p7m` non sono leggibili direttamente. È necessario estrarre prima il file `.xml` interno, ad esempio tramite:
  - [Dike 6](https://www.infocert.it/servizi/dike/) o altri software di firma digitale
  - Strumenti da linea di comando: `openssl cms -verify -noverify -in fattura.xml.p7m -inform DER -out fattura.xml`
- **File con firma XAdES embedded:** alcune fatture SdI hanno firma XML digitale integrata direttamente nel file `.xml`. Questi file sono compatibili e vengono aperti correttamente (la firma viene ignorata durante il parsing).

---

## Requisiti tecnici

Nessuna dipendenza esterna. Il file `index.html` è completamente autocontenuto.

**Browser supportati:** qualsiasi browser moderno con supporto a `FileReader`, `DOMParser`, `Blob URL`.

- Chrome / Chromium 80+
- Firefox 75+
- Safari 14+
- Edge 80+

---

## Struttura del progetto

```
index.html    ← l'intera applicazione (HTML + CSS + JS in un unico file)
README.md     ← questo file
```

---

## Contribuire

Chiunque può contribuire al progetto. Pull request, segnalazioni di bug e suggerimenti sono benvenuti.

Alcune idee su come aiutare:
- Testare il visualizzatore con fatture reali e segnalare eventuali anomalie
- Aggiungere il supporto a campi FatturaPA non ancora visualizzati
- Migliorare l'interfaccia grafica o l'accessibilità
- Segnalare bug aprendo una [issue](../../issues)

Trattandosi di un singolo file HTML senza dipendenze, contribuire è semplice: basta modificare `index.html` e aprire una pull request.

---

## Specifiche di riferimento

- [Specifiche tecniche FatturaPA v1.2](https://www.fatturapa.gov.it/it/norme-e-regole/documentazione-fattura-elettronica/)
- Namespace XML: `http://ivaservizi.agenziaentrate.gov.it/docs/xsd/fatture/v1.2`
- Agenzia delle Entrate, Sistema di Interscambio (SdI)
