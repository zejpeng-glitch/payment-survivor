# Account: eliminazione e recupero password

## Elimina account (Impostazioni)
Edge Function `delete-account` (già pubblicata su Supabase). Cancella le scansioni in `scans/<id utente>/`
e l'utente; profilo, scadenze, multe, bollette e abbonamenti seguono in cascata.

```ts
const { error } = await supabase.functions.invoke('delete-account');
if (!error) await supabase.auth.signOut();
```
Testo suggerito: «Se vuoi andare, nessun problema. Cancelliamo il tuo account e i tuoi dati. Ci vuole un attimo.»
Chiedi una conferma amichevole prima di procedere, senza toni di colpa.

## Password dimenticata (schermata di accesso)
```ts
await supabase.auth.resetPasswordForEmail(email, { redirectTo: 'paymentsurvivor://reset-password' });
// nella schermata reset-password, dopo il deep link:
await supabase.auth.updateUser({ password: nuovaPassword });
```
Da configurare in Supabase → Authentication → URL Configuration: aggiungere `paymentsurvivor://reset-password`
tra i Redirect URLs, e impostare `"scheme": "paymentsurvivor"` in app.json.
