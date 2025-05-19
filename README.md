# ESERCIZIO LINUX SERVER
### Script e Cron
---

### Scope:
Creare uno script che faccia il backup della mia cartella home in una cartella che è sotto opt backup.  Il backup è necessario che sia fatto giornalmente e solo una settimana in linea (devono esserci al massimo 7 file).

## Step 1: creazione della tabella backup

````
sudo mkdir -p /opt/backup
````
Crea la directory `/opt/backup` se non esiste già.
Il flag `-p` permette di creare anche directory intermedie.

````
sudo chown root:root /opt/backup
sudo chmod 700 /opt/backup
````
Il comando `sudo chown` cambia l'utente proprietario e il gruppo associato a un file o cartella.
In questo modo altri utenti non avranno i permessi.


## Step 2: programmazione del Cron

Pianificazione con Cron
````
sudo crontab -e
````
Questo comando apre il file della pianificazione dei comandi automatici per l'utente root.


Eseguiamo il bacckup giornaliero alle 3:00

````
0 3 * * * /bin/bash -c 'tar -czf /opt/backup/backup_home_$(date +\%Y\%m\%d).tar.gz /home'
````
- `0 3 * * * ` indica l'orario in cui viene eseguito il comando, che corrisponde alle 03:00. Il primo numero indica i minuti, il secondo l'ora, il terzo il giorno del mese, il quarto il mese e l'ultimo il giorno della settimana.
- `/bin/bash -c 'tar -czf /opt/backup/backup_home_$(date +\%Y\%m\%d).tar.gz /home'` questo esegue il comando tra virgolette in una shell bash.
- `tar -czf` serve per creare un archivio `.tar.gz`. Perciò `-c` crea, `-z` comprime, `-f` salva su un file con nome specifico.
- `/opt/backup/backup_home_$(date +\%Y\%m\%d).tar.gz` è il nome del file di backup.
- `/home` è la cartella da salvare nel backup.


Eliminiamo i backup più vecchi di 7 giorni

````
0 2 * * * /bin/bash -c 'find /opt/backup -name "backup_home_*.tar.gz" -mtime +6 -delete'
````
- `0 2 * * * ` indica l'orario in cui viene eseguito il comando, cioè alle 02:00.
- `-mtime +6` seleziona i file modificati più di 6 giorni fa.
- `delete` elimina i file trovati.
