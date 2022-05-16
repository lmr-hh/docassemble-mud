<div class="alert alert-info" role="alert">
  <h5 class="alert-heading">Testmodus</h5>

  Die Anmeldung befindet sich im Testmodus. Daher werden keine E-Mails
  verschickt und auch keine weiteren Automatisierungen vorgenommen. Dies ist nur
  eine Zusammenfassung der Automatisierung, die bei einer echten Anmeldung
  ablaufen würde.
</div>

### Anmeldungsformular und Rechnung

Es wurde ein Anmeldungsformular als PDF generiert.

<p>
  <a href="${ anmeldung.pdf.url_for() }"
    target="_blank"
    class="btn btn-primary btn-sm">
      Anmeldeformular ansehen
  </a>
  <a href="${ rechnung.pdf.url_for() }"
    target="_blank"
    class="btn btn-primary btn-sm">
      Rechnung ansehen
  </a>
</p>

### Begrüßungs-E-Mail
Eine E-Mail würde an `${ person.email }` geschickt werden.
Der Text der E-Mail kann unten angesehen werden. Das Design der
E-Mail sieht allerdings anders aus.

An die E-Mail sind die folgenden Dokumente angehängt:

- [${ anmeldeformular.pdf.filename }](${ anmeldung.pdf.url_for() })
- [${ rechnung.pdf.filename }](${ rechnung.pdf.url_for() })

<p>
  <%self:collapse_button id="person-email-collapse">
    E-Mail-Inhalt anzeigen
  </%self:collapse_button>
  <%self:action_button action="send_member_email"
                       message="Die E-Mail wurde gesendet. Es kann einen Moment dauern, bis die Mail ankommt.">
    E-Mail trotzdem senden
  </%self:action_button>
</p>

<%self:collapse id="person-email-collapse" title="${ person_email.subject }">
  ${ person_email }
</%self:collapse>

### Benachrichtigungs-E-Mail
Bei jeder Anmeldung werden bestimmte Empfänger benachrichtigt.
% if not daten["E-Mail Benachrichtigung"]:
Es sind bisher keine Empfänger konfiguriert, daher würde dieser Schritt
übersprungen werden.
% else:
Die Benachrichtigung enthält nur wenig Text. Folgende Empfänger werden benachrichtigt:

% for email in daten["E-Mail Benachrichtigung"]:
  - `${ email }`
% endfor

Wenn du unten auf "E-Mail trotzdem senden" klickst, wird die E-Mail im Testmodus
nur an die von dir angegebene Adresse `${ person.email }` gesendet.

<p>
  <%self:collapse_button id="orga-email-collapse">
    E-Mail-Inhalt anzeigen
  </%self:collapse_button>
  <%self:action_button action="send_orga_email"
                       message="Die E-Mail wurde gesendet. Es kann einen Moment dauern, bis die E-Mail ankommt.">
    E-Mail trotzdem senden
  </%self:action_button>
</p>

<%self:collapse id="orga-email-collapse" title="${ orga_email.subject }">
  ${ orga_email }
</%self:collapse>
% endif

### Archivieren der Anmeldung und Rechnung
Das Anmeldeformular und die Rechnung werden automatisch in dem Ordner
**${ ordner['name'] }** archiviert.
% if not discount_proof:
In der Anmeldung wurde kein Ermäßigungsnachweis hochgeladen.
% else:
Auch der Ermäßigungsnachweis wird in diesem Ordner archiviert.
% endif

<p>
  <%self:action_button action="archive_registration"
                       message="Die Anmeldung wurde zum Ordner hinzugefügt.">
    Anmeldung archivieren
  </%self:action_button>
  <%self:action_button action="archive_invoice"
                       message="Die Rechnung wurde zum Ordner hinzugefügt.">
    Rechnung archivieren
  </%self:action_button>
% if discount_proof:
  <%self:action_button action="archive_discount_proof"
                       message="Der Nachweis wurde zum Ordner hinzugefügt.">
    Ermäßigungsnachweis archivieren
  </%self:action_button>
% endif
  <a class="btn btn-secondary btn-sm"
     target="_blank"
     href="${ ordner['webUrl'] }">Ordner öffnen</a>
</p>

### Anmeldeliste
Alle eingegebenen Daten werden automatisch zur Anmeldeliste hinzugefügt. Die
Anmeldeliste ist eine Excel-Datei mit Namen
`${ tabelle["datei"]["name"] }`. Die Daten werden dort der Tabelle
`${ tabelle["tabelle"]["name"] }` hinzugefügt. Überschriften werden
automatisch erkannt und den Einträgen zugeordnet.

<p>
  <%self:action_button action="append_to_table"
                       message="Die Daten wurden zur Anmeldeliste hinzugefügt.">
    Zur Tabelle hinzufügen
  </%self:action_button>
  <a class="btn btn-secondary btn-sm"
     target="_blank"
     href="${ tabelle['datei']['webUrl'] }">Tabelle öffnen</a>
</p>
